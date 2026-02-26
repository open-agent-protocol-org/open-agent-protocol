# Open Agent Protocol (OAP)

Open Agent Protocol (OAP) is an open, vendor-neutral specification for defining, packaging, and running AI agents.

OAP aims to establish a common foundation for agent manifests, permissions, tools, memory, and runtime compatibility — independent of model providers and platforms.

---

## Why OAP?

AI agents are rapidly evolving, but the ecosystem is fragmented.

Today:
- Agent definitions differ across platforms
- Permissions are inconsistently structured
- Tool integrations are platform-specific
- Runtimes are not interoperable
- Vendor lock-in is increasing

There is no universal standard for AI agents.

OAP exists to change that.

---

## Goals

- Enable agent portability across runtimes
- Reduce vendor lock-in
- Standardize permission declaration and enforcement
- Define a consistent manifest structure
- Provide runtime compliance guidelines
- Support long-term interoperability

Build once. Run anywhere.

---

## Core Components

OAP v0.1 defines:

- **Manifest Specification**  
  Standardized `manifest.json` structure

- **Permission Taxonomy**  
  Unified, structured permission model

- **Runtime Compliance Requirements**  
  Minimum requirements for OAP-compatible runtimes

- **Tool Interface Specification**  
  How agents declare and access external tools

- **Memory Declaration Model**  
  How agents define memory scope and retention

See `/spec` for detailed documentation.

---

## Current Status

OAP v0.1 — Draft Specification

This is an early-stage open initiative.
The specification is evolving and open for feedback.

---

## Repository Structure
/spec
manifest.md
permissions.md
runtime.md
tools.md
memory.md

/examples
daily-planner-agent
finance-agent

MANIFESTO.md
roadmap.md
CONTRIBUTING.md

---

## Contributing

Contributions are welcome.

You can:
- Propose improvements via Pull Requests
- Open issues for discussion
- Add example agents
- Suggest improvements to the permission model
- Help refine runtime compliance

See `CONTRIBUTING.md` for details.

---

## Long-Term Vision

OAP aims to become a foundational standard for AI agents,
similar to how HTTP enabled the web.

The goal is a future where:

- Agents are portable
- Runtimes are interoperable
- Permissions are transparent
- Ecosystems are open

---

## License

Apache 2.0
