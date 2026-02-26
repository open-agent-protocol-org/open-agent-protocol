# OAP Agent Packaging & Signing Specification (Draft v0.3)

## 1. Purpose

This document defines how OAP agents are packaged, versioned, and optionally signed.

The objective is to ensure:

- Integrity
- Portability
- Deterministic distribution
- Secure marketplace compatibility

---

## 2. Agent Package Structure

An OAP agent package MUST contain:

- manifest.json
- optional README.md
- optional assets/
- optional tools/
- optional additional metadata files

Minimal required structure:

```
my-agent/
  manifest.json
```

Extended example:

```
my-agent/
  manifest.json
  README.md
  assets/
  tools/
```

---

## 3. Packaging Format

An agent package MAY be distributed as:

- Directory
- Compressed archive (.zip)
- Future: .oap package format (recommended)

If archived, the root directory MUST contain manifest.json.

---

## 4. Versioning

Agents MUST follow semantic versioning:

```
MAJOR.MINOR.PATCH
```

Rules:

- MAJOR: breaking changes
- MINOR: backward-compatible additions
- PATCH: bug fixes or metadata updates

Runtimes MUST compare versions on update.

---

## 5. Integrity Verification

Runtimes SHOULD verify agent integrity before execution.

Recommended approaches:

- SHA256 checksum
- Signed package (see Section 6)

If integrity validation fails, runtime MUST reject execution.

---

## 6. Optional Agent Signing

Agents MAY be cryptographically signed.

Signing enables:

- Identity verification
- Tamper detection
- Marketplace trust models

### Recommended Model

- Developer generates public/private key pair
- Package is hashed
- Hash is signed with private key
- Signature stored alongside package

Example structure:

```
my-agent/
  manifest.json
  signature.sig
  public_key.pem
```

Runtimes MAY:

- Verify signature before execution
- Display verified publisher identity
- Warn users for unsigned agents

---

## 7. Publisher Identity

Future versions MAY support:

- Verified publisher registry
- Public key directory
- Reputation scoring
- Revocation lists

---

## 8. Update Rules

When an agent updates:

- Runtime MUST compare versions
- Runtime MUST detect permission changes
- Runtime MUST re-prompt user if new permissions are added
- Runtime SHOULD verify signature consistency

---

## 9. Marketplace Compatibility

For future marketplace implementations:

- Agents SHOULD be signed
- Integrity MUST be verifiable
- Version history SHOULD be preserved
- Publisher identity SHOULD be transparent

---

## 10. Future Extensions

- Official .oap binary packaging format
- Deterministic package hashing
- Supply chain security standards
- Multi-signer support
- Enterprise signing authorities
