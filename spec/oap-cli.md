# OAP CLI Specification (Draft v0.2)

## 1. Purpose

This document defines the conceptual command-line interface (CLI) for interacting with OAP agents.

The OAP CLI is intended to:

- Validate manifests
- Inspect agent packages
- Assist developers in creating OAP-compliant agents
- Provide automated compliance checks

This document defines behavior only. It does not define implementation.

---

## 2. Command: oap validate

Validate an agent manifest against the official JSON schema.

Example:

    oap validate manifest.json

Behavior:

- Validates against manifest.schema.json
- Outputs structured validation results
- Returns exit code 0 if valid
- Returns non-zero exit code if invalid

Optional flags:

    --strict
    --schema-version

---

## 3. Command: oap lint

Provides additional non-fatal warnings and best-practice checks.

Example:

    oap lint manifest.json

Checks MAY include:

- Missing recommended fields
- Overly broad permissions
- Missing descriptions
- Trigger misuse
- Version formatting issues

Linting does not block execution.

---

## 4. Command: oap inspect

Displays human-readable summary of an agent package.

Example:

    oap inspect ./agent/

Output SHOULD include:

- Agent ID
- Version
- Declared permissions
- Risk level summary
- Supported models
- Trigger types
- Memory configuration

---

## 5. Command: oap init

Generates a scaffold for a new OAP agent.

Example:

    oap init my-agent

Creates:

- manifest.json template
- README.md
- Example folder structure

---

## 6. Future Extensions

Future versions MAY include:

- oap test (compliance validation)
- oap pack (agent packaging)
- oap publish (registry integration)
- oap sign (agent signing & integrity verification)

---

## 7. Design Principles

The OAP CLI MUST:

- Be runtime-neutral
- Be model-agnostic
- Use the official JSON schema for validation
- Provide deterministic output
- Support CI/CD integration

---

## 8. Relationship to Compliance

The CLI MAY be used by runtimes to:

- Validate agent packages
- Automate compliance verification
- Integrate into build pipelines

The CLI does not replace runtime enforcement.
It complements it.
