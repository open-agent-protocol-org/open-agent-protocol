# OAP Runtime Compliance Checklist (Draft v0.2)

## 1. Purpose

This document defines the minimum requirements for a runtime to claim OAP compatibility.

A runtime is any environment that loads an OAP agent package and executes the agent (cloud, desktop, mobile, embedded, etc.).

---

## 2. Compliance Levels

OAP defines two compliance levels:

- **Core Compliance (Required)**  
  Minimum required behavior to support OAP agents safely and predictably.

- **Extended Compliance (Optional)**  
  Additional capabilities that improve portability and ecosystem maturity.

Runtimes MAY claim "OAP Core" or "OAP Extended" compliance.

---

## 3. Core Compliance Requirements (Required)

### 3.1 Manifest Parsing

Runtime MUST:

- Load and parse `manifest.json`
- Validate required fields
- Reject execution if required fields are missing or invalid
- Validate `oap_version` compatibility

### 3.2 Permission Handling

Runtime MUST:

- Display all declared permissions prior to installation or first run
- Require explicit user approval for requested permissions
- Enforce permissions at execution time (block undeclared actions)
- Prevent silent permission escalation on updates (must re-prompt)

### 3.3 Model / Provider Compatibility

Runtime MUST:

- Allow selecting a compatible model from `runtime_compatibility.models_supported`
- Fail gracefully if no compatible model is available
- Record which model/provider was used for an execution

### 3.4 Trigger Execution Support

Runtime MUST:

- Support **manual triggers**
- Respect trigger metadata injection requirements (see `events.md`)
- Log trigger type for each execution

### 3.5 Tool Invocation Enforcement

Runtime MUST:

- Only allow tools declared by the agent package/manifest
- Validate permissions required for each tool call
- Block tool calls requiring unapproved permissions
- Provide tool call results back into the agent execution context

### 3.6 Memory Scope Enforcement

If memory is enabled, runtime MUST:

- Respect declared memory scope (per_user / per_workspace)
- Respect retention policy (if defined)
- Prevent cross-user memory leakage

### 3.7 Execution Logging (Minimum)

Runtime MUST log at minimum:

- Execution ID
- Agent ID
- Timestamp
- Trigger type
- Tools invoked (names)
- Success / failure result

### 3.8 Fail-Safe Behavior

Runtime MUST:

- Stop execution on permission denial
- Stop execution if manifest validation fails
- Stop execution if tool validation fails
- Provide a clear error output to the user

---

## 4. Extended Compliance Requirements (Optional)

### 4.1 Scheduled Triggers

Runtime SHOULD:

- Support scheduled triggers (`cron`)
- Allow user modification/disable of schedules
- Log scheduled executions

### 4.2 Event-Based Triggers

Runtime SHOULD:

- Support event triggers
- Allow per-trigger enable/disable
- Enforce background execution permission

### 4.3 Permission Risk UX

Runtime SHOULD:

- Display permissions grouped by risk level (Low/Medium/High)
- Provide human-readable explanations for each permission
- Offer easy permission revocation UI

### 4.4 Audit Log Detail

Runtime SHOULD log:

- Permission usage events
- Tool inputs/outputs (redacted when necessary)
- Execution duration
- Retry attempts and failures

### 4.5 Manifest Validation Schema

Runtime SHOULD:

- Validate manifest using a published JSON Schema
- Provide clear error messages for invalid fields

---

## 5. Compliance Claim Format (Recommended)

Runtimes claiming compliance SHOULD publish:

- Supported OAP version(s)
- Compliance level (Core / Extended)
- Supported model providers
- Supported trigger types
- Any known limitations

Example:

- OAP: v0.2
- Compliance: Core
- Providers: openai, anthropic, local
- Triggers: manual
- Limitations: no scheduled/event triggers

---

## 6. Future Work

Future versions MAY include:

- Official compliance test suite
- Automated runtime certification
- Compatibility badges for marketplaces
