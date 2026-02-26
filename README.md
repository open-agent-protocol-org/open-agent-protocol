# Open Agent Protocol (OAP)

**The open standard for portable AI agents.**

Open Agent Protocol (OAP) defines a vendor-neutral way to package, distribute, and run AI agents across platforms, runtimes, and devices.

OAP is not tied to any single model provider, company, device, or ecosystem.  
It aims to become the universal distribution and interoperability layer for AI agents.

---

## 🎯 Vision

AI agents should be:

- Portable across runtimes
- Explicit about permissions
- Safe by default
- Vendor-neutral
- Easy to distribute

OAP standardizes how agents are described, packaged, validated, installed, and executed.

---

## 📦 What OAP Defines

OAP is a protocol specification consisting of:

### 1. Agent Package Format (`.oap`)
A portable ZIP-based archive containing:
- `manifest.json`
- optional resources
- optional metadata

### 2. Agent Manifest
Defines:
- agent identity
- version
- permissions
- required tools
- compatibility constraints

### 3. Permissions Model
Agents must explicitly declare required permissions.
Runtimes must prompt users and enforce access.

### 4. Tool Allowlisting
Agents can only call tools declared in their manifest.
Runtimes must enforce tool access.

### 5. Registry Model
Defines how agents can be:
- published
- discovered
- installed
- versioned

---

## 📚 Specifications

All canonical specifications live in the `/spec` directory:

- `spec/oap-v0.2.md` — Core protocol specification (Draft)

Future specs may include:
- Registry specification
- Package signing & trust model
- Governance model
- Security guidelines

---

## 🧪 Reference Implementation

This repository defines the protocol.

A working reference runtime + CLI lives here:

👉 https://github.com/open-agent-protocol-org/oap-reference-runtime

The reference implementation includes:
- CLI tooling (`oap ...`)
- runtime enforcement
- packaging system
- local registry
- audit logging
- example agents

---

## 🚧 Status

- Protocol maturity: Draft
- Current version: v0.2
- Breaking changes possible before v1.0
- Goal: stabilize specification toward v1.0

---

## 🤝 Contributing

OAP is an open protocol initiative.

You can contribute by:
- Proposing spec improvements
- Submitting clarifications
- Improving the registry model
- Building compatible runtimes
- Reviewing security models

Formal governance will be defined as the ecosystem grows.

---

## 🛡 License

Licensed under the Apache License 2.0.  
See the LICENSE file for details.