# Open Agent Protocol (OAP) — Registry Specification v0.1 (Draft)

**Status:** Draft  
**Version:** 0.1  
**Last updated:** 2026-02-28  
**Scope:** Defines the registry index format, package addressing, integrity fields, and minimum HTTP hosting expectations for OAP registries.

> This document specifies the **Registry Layer** used to publish, discover, and install OAP agent packages (`.oap`).  
> The canonical agent format and manifest are defined in: `spec/oap-v0.2.md`.

---

## 1. Purpose

An OAP Registry enables:

- Publishing agent packages (`.oap`)
- Discovery and search via a structured index
- Versioning of agents and packages
- Integrity verification via checksums (SHA-256)
- Portable hosting (local folder, static hosting, HTTP server)

A registry is **vendor-neutral** and can be:
- Local (filesystem)
- Private (company internal)
- Public (community hosted)
- Mirrored (cached/replicated index + packages)

---

## 2. Terminology

- **Registry Root:** The filesystem directory or HTTP base URL that contains the registry index and packages.
- **Index:** A JSON document (default: `index.json`) listing agents and versions.
- **Package:** A `.oap` ZIP container file holding an agent (`manifest.json` required).
- **Publisher:** Metadata about the entity that published an agent/version (identity and signing are optional in v0.1).
- **Download URL:** A relative or absolute URL/path to fetch a package.

---

## 3. Registry Root Layout (Filesystem)

A filesystem registry SHOULD have this layout:

~~~text
registry/
├── index.json
└── packages/
    ├── com.example.agent-0.1.0.oap
    └── com.example.agent-0.2.0.oap
~~~

### 3.1 Required paths
- `index.json` MUST exist at the registry root.
- `packages/` SHOULD exist (recommended). Registries MAY store packages elsewhere if `download_url` is absolute.

---

## 4. Registry Index Format (`index.json`)

### 4.1 Top-level schema (normative)

The registry index MUST be valid JSON and contain:

- `registry_version` (string) — index format version (this spec: `"0.1"`)
- `generated_at` (string) — ISO8601 timestamp
- `agents` (array) — list of agent entries

#### Required fields
- `registry_version`
- `generated_at`
- `agents`

#### Example (minimal)
~~~json
{
  "registry_version": "0.1",
  "generated_at": "2026-02-28T12:00:00.000Z",
  "agents": []
}
~~~

---

## 5. Agent Entry

Each element in `agents[]` MUST be an object with:

- `agent_id` (string)
- `name` (string)
- `description` (string)
- `latest_version` (string)
- `versions` (object map of version → version entry)

It MAY include:

- `publisher` (object)
- `tags` (string[])
- `links` (object)
- `created_at` / `updated_at` (string ISO8601)

### 5.1 Agent Entry example
~~~json
{
  "agent_id": "com.oap.myfirst",
  "name": "My First Agent",
  "description": "Describe what this agent does.",
  "latest_version": "0.1.0",
  "publisher": {
    "display_name": "Anonymous",
    "publisher_id": "pub_anon_001",
    "public_key_ed25519": "BASE64..."
  },
  "versions": {
    "0.1.0": {
      "package": {
        "filename": "com.oap.myfirst-0.1.0.oap",
        "sha256": "94f33957178fecfd3161887991296b707f3550543377df1337b653f0def92b57",
        "size_bytes": 719,
        "download_url": "packages/com.oap.myfirst-0.1.0.oap"
      },
      "manifest": {
        "oap_version": "0.2",
        "agent_id": "com.oap.myfirst",
        "version": "0.1.0",
        "permissions": ["notifications.send"],
        "tools": ["tools.mock_echo"]
      },
      "signature": {
        "alg": "ed25519",
        "signed_at": "2026-02-28T12:00:00.000Z",
        "signature": "TBD"
      }
    }
  }
}
~~~

---

## 6. Version Entry

Each `versions[<version>]` MUST be an object with:

- `package` (object)
- `manifest` (object snapshot)

It MAY include:
- `signature` (object, optional in v0.1)
- `released_at` (string ISO8601)
- `yanked` (boolean) / `deprecated` (boolean)
- `notes` (string)

### 6.1 Why include a manifest snapshot?
Including a manifest snapshot in the index:
- Enables fast search/filter without downloading packages
- Improves integrity checking and auditing
- Allows registries to display metadata without extraction

The snapshot SHOULD include at least:
- `oap_version`
- `agent_id`
- `version`
- `permissions`
- `tools` (if present)

---

## 7. Package Object

The `package` object MUST include:

- `sha256` (string) — lowercase hex-encoded SHA-256 digest of the `.oap` file contents
- `download_url` (string) — relative path or absolute URL
- `size_bytes` (number) — size in bytes

It SHOULD include:
- `filename` (string)

### 7.1 Package Object example
~~~json
{
  "filename": "com.example.agent-0.1.0.oap",
  "sha256": "hex...",
  "size_bytes": 12345,
  "download_url": "packages/com.example.agent-0.1.0.oap"
}
~~~

---

## 8. Publisher Object (Optional in v0.1)

A `publisher` object MAY be included in an agent entry.

Fields:
- `display_name` (string)
- `publisher_id` (string)
- `public_key_ed25519` (string, base64) — optional in v0.1, recommended for v0.2+

### 8.1 Notes
- In v0.1, publisher identity is informational.
- Signing and trust enforcement are planned for later versions.

---

## 9. Signature Object (Optional in v0.1)

A version entry MAY include a `signature` object:

- `alg` (string) — e.g. `"ed25519"`
- `signed_at` (string ISO8601)
- `signature` (string) — base64 signature

### 9.1 v0.1 behavior
- Runtimes MAY ignore `signature`.
- Registries MAY set `"signature": "TBD"` while scaffolding.
- Enforcement is out of scope for v0.1.

---

## 10. Discovery and Search Semantics (Index-Based)

### 10.1 Basic search
A client SHOULD implement search over:
- `agent_id`
- `name`
- `description`
- optional `tags`

Search is implementation-defined (substring match, token match, etc.) in v0.1.

### 10.2 Version selection
A client MAY:
- default to `latest_version`
- allow explicit version selection
- reject versions marked as `yanked` or `deprecated` (if present)

---

## 11. Installation Requirements (Client/Runtime)

An OAP runtime/client installing from a registry MUST:

1. Fetch `index.json` (or read local file)
2. Locate the agent and desired version
3. Fetch the package from `package.download_url`
4. Verify the `.oap` file SHA-256 matches `package.sha256`
5. Unpack into local store path:
   - `agents/<agent_id>/<version>/`
6. Validate `manifest.json` using OAP spec
7. Refuse install if:
   - checksum mismatch
   - manifest invalid
   - required fields missing

---

## 12. HTTP Hosting Model (Minimum)

A registry MAY be hosted over HTTP. The minimum viable HTTP registry is **static hosting**.

### 12.1 Required endpoints (static)
A registry hosted at base URL `https://registry.example.com/` SHOULD serve:

- `GET /index.json`
- `GET /packages/<filename>.oap` (or any path referenced by `download_url`)

### 12.2 Content types
Servers SHOULD set:
- `index.json` → `application/json`
- `.oap` → `application/octet-stream`

### 12.3 CORS
Public registries SHOULD enable CORS for `GET` requests to support browser-based clients.

---

## 13. Mirroring and Caching (Non-normative)

Registries may be mirrored by:
- copying `index.json` and `packages/`
- caching packages on demand
- rewriting `download_url` for local availability

This spec does not mandate mirroring behavior.

---

## 14. Security Considerations

### 14.1 Integrity
SHA-256 verification is mandatory for clients in this spec.

### 14.2 Authenticity (future)
Registry entries MAY include publisher keys and signatures, but v0.1 does not enforce them.

### 14.3 Malicious packages
Clients SHOULD treat `.oap` as untrusted input:
- validate manifest strictly
- ignore unexpected files
- enforce tool allowlist and permissions at runtime

---

## 15. Compliance Summary

### Registry MUST
- Provide `index.json` with required fields
- Provide version entries with `package.sha256`, `package.size_bytes`, and `package.download_url`

### Registry SHOULD
- Store packages under `packages/`
- Provide `filename`
- Include `manifest` snapshots
- Include `publisher` metadata

### Client/Runtime MUST
- Verify SHA-256 before install
- Validate manifest after install
- Store agents by `agent_id` + `version`

---

## Appendix A — Suggested JSON Schema (Informative)

A JSON schema for `index.json` is recommended but not required in v0.1.

---

## Appendix B — Example Full `index.json`

~~~json
{
  "registry_version": "0.1",
  "generated_at": "2026-02-28T12:00:00.000Z",
  "agents": [
    {
      "agent_id": "com.oap.myfirst",
      "name": "My First Agent",
      "description": "Describe what this agent does.",
      "publisher": {
        "display_name": "Anonymous",
        "publisher_id": "pub_anon_001",
        "public_key_ed25519": "BASE64..."
      },
      "latest_version": "0.1.0",
      "versions": {
        "0.1.0": {
          "package": {
            "filename": "com.oap.myfirst-0.1.0.oap",
            "sha256": "94f33957178fecfd3161887991296b707f3550543377df1337b653f0def92b57",
            "size_bytes": 719,
            "download_url": "packages/com.oap.myfirst-0.1.0.oap"
          },
          "manifest": {
            "oap_version": "0.2",
            "agent_id": "com.oap.myfirst",
            "version": "0.1.0",
            "permissions": ["notifications.send"],
            "tools": ["tools.mock_echo"]
          },
          "signature": {
            "alg": "ed25519",
            "signed_at": "2026-02-28T12:00:00.000Z",
            "signature": "TBD"
          }
        }
      }
    }
  ]
}
~~~