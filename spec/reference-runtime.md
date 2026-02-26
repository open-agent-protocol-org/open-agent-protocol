# OAP Reference Runtime Architecture (Draft v0.2)

## 1. Purpose

This document describes a minimal "reference runtime" architecture for executing OAP agents.

The reference runtime is not a required implementation, but a recommended baseline design that:
- Proves the protocol is implementable
- Helps runtime builders align on shared concepts
- Provides a canonical interpretation of OAP lifecycle, permissions, tools, memory, and logging

---

## 2. Non-Goals

The reference runtime does NOT aim to:
- Provide a full marketplace
- Provide enterprise IAM/SSO features
- Solve all sandboxing and isolation problems
- Support every tool type or deployment model

The focus is a minimal, interoperable execution baseline.

---

## 3. High-Level Components

An OAP reference runtime consists of:

1. Agent Loader
2. Manifest Validator
3. Permission Manager
4. Execution Engine
5. Tool Router
6. Memory Store
7. Audit Logger
8. Policy / Limits Layer

---

## 4. Component Responsibilities

### 4.1 Agent Loader

- Loads agent package from local path or registry source
- Ensures required files exist (at minimum `manifest.json`)
- Exposes the agent metadata to the runtime

Inputs:
- Agent package path (e.g., `./agent/`)

Outputs:
- Parsed package structure + manifest content

---

### 4.2 Manifest Validator

- Validates `manifest.json` against the official JSON schema
- Validates internal consistency:
  - Triggers require background permission
  - Tools reference declared permissions
  - Version format sanity checks
- Rejects execution if invalid

Outputs:
- Pass/fail + validation errors

---

### 4.3 Permission Manager

- Displays declared permissions to user prior to install/first run
- Stores user approval state per agent and per permission
- Enforces permission checks at runtime:
  - Blocks tool calls without approved permissions
  - Blocks undeclared permission usage
- Supports revocation and re-prompting on updates

Notes:
- Uses risk model for UX behavior (Low/Medium/High)

---

### 4.4 Execution Engine

Implements the OAP lifecycle phases (see `lifecycle.md`):

1. Initialization
2. Permission Validation
3. Context Loading
4. Execution
5. Tool Invocation
6. Memory Update
7. Completion / Error
8. Logging

Responsibilities:
- Selects a compatible model/provider
- Constructs system + user context
- Maintains an execution ID
- Applies runtime limits (timeouts, max steps, etc.)
- Produces a final response or structured error

---

### 4.5 Tool Router

- Resolves tool names to concrete tool implementations
- Enforces tool-level permission requirements
- Executes tool calls and returns results to the execution engine
- Redacts sensitive tool outputs when required (policy-dependent)

Tool types (minimal baseline):
- Built-in tools (e.g., web browse, simple search)
- HTTP tools (API calls)

---

### 4.6 Memory Store

If `memory.enabled = true`, the runtime stores user-scoped agent memory.

Responsibilities:
- Store memory per declared scope (per_user / per_workspace)
- Enforce retention policy when defined
- Prevent cross-user leakage
- Support retrieval during context loading

Baseline memory format:
- Free-text summaries
- Optional structured entries (future extension)

---

### 4.7 Audit Logger

Provides traceability and debugging.

Minimum required logs (Core Compliance):
- Execution ID
- Agent ID
- Timestamp
- Trigger type
- Tools invoked (names)
- Success / failure status

Recommended additional logs:
- Permission usage events
- Execution duration
- Model/provider used
- Error codes and stack traces (redacted if needed)

---

### 4.8 Policy / Limits Layer

Defines runtime constraints and safety boundaries.

Examples:
- Max execution duration
- Max tool calls per run
- Blocked domains or restricted tools
- High-risk permission gating rules
- Background execution restrictions

This layer is runtime-specific but MUST not break OAP compliance.

---

## 5. Reference Runtime Execution Flow

A typical manual execution:

1. Agent Loader loads package
2. Manifest Validator validates manifest
3. Permission Manager ensures permissions are approved
4. Execution Engine starts run and assigns execution_id
5. Execution Engine requests tool calls (if needed)
6. Tool Router executes calls (permission-checked)
7. Execution Engine returns response
8. Memory Store updates memory (if enabled)
9. Audit Logger records execution

---

## 6. Minimal Data Structures (Recommended)

### 6.1 Execution Context

Runtimes SHOULD maintain:

- execution_id
- agent_id
- trigger_type
- trigger_id (optional)
- timestamp
- model/provider used

### 6.2 Tool Call Envelope

Tool calls SHOULD include:

- tool_name
- input payload
- permissions_required
- execution_id

---

## 7. Deployment Modes (Non-Normative)

The reference runtime can be implemented as:

- Web runtime (fastest to prototype)
- Desktop runtime (better background support)
- Local runtime (privacy-first)
- Cloud runtime (managed hosting)

OAP itself remains runtime-neutral.

---

## 8. Future Extensions

- Compliance test suite integration
- Agent signing and integrity verification
- Sandboxed execution environments
- Multi-agent orchestration
- Deterministic replay of agent runs
- Standardized tool descriptor formats (OpenAPI)

---
