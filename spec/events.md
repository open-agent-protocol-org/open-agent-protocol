# OAP Event & Trigger Model (Draft v0.2)

## 1. Purpose

This document defines how OAP-compliant agents are triggered and executed.

The goal is to standardize activation behavior across runtimes and ensure predictable, auditable execution flows.

---

## 2. Trigger Categories

An OAP agent MAY declare one or more of the following trigger types:

- Manual
- Scheduled
- Event-Based

If no trigger is declared, manual invocation is assumed.

---

## 3. Manifest Structure

Triggers MUST be declared inside the agent manifest:

```
{
  "triggers": {
    "manual": true,
    "scheduled": [],
    "events": []
  }
}
```

Each trigger type is defined below.

---

## 4. Manual Trigger

Manual triggers allow explicit user invocation.

Example:

```
{
  "triggers": {
    "manual": true
  }
}
```

### Runtime Requirements

- Must allow explicit user invocation
- Must log manual executions
- Must inject execution metadata
- Must enforce permission checks

---

## 5. Scheduled Trigger

Scheduled triggers allow agents to execute at defined intervals.

Example:

```
{
  "triggers": {
    "scheduled": [
      {
        "id": "daily_summary",
        "cron": "0 7 * * *",
        "description": "Runs every day at 07:00"
      }
    ]
  }
}
```

### Required Fields

- id
- cron

### Optional Fields

- description

### Runtime Requirements

- Must allow user modification of schedule
- Must allow disabling schedules
- Must log scheduled executions
- Must inject trigger metadata
- Must enforce background execution permission

---

## 6. Event-Based Trigger

Event triggers allow agents to respond to external or system-generated events.

Example:

```
{
  "triggers": {
    "events": [
      {
        "id": "new_email",
        "source": "email",
        "event_type": "message.received",
        "filter": {
          "from_domain": "company.com"
        }
      }
    ]
  }
}
```

### Required Fields

- id
- source
- event_type

### Optional Fields

- filter
- debounce

### Runtime Responsibilities

- Validate required permissions
- Allow enabling/disabling individual triggers
- Log event-driven executions
- Enforce permission boundaries

---

## 7. Background Execution

Agents declaring scheduled or event triggers implicitly require:

```
"system.background_execution"
```

Runtimes MUST:

- Explicitly request user consent
- Clearly indicate background behavior
- Allow users to revoke background execution

---

## 8. Execution Metadata Injection

During execution, runtimes MUST inject structured metadata into the agent context.

Example:

```
{
  "execution_context": {
    "trigger_type": "manual | scheduled | event",
    "trigger_id": "<trigger_id>",
    "timestamp": "<ISO8601 timestamp>"
  }
}
```

This ensures:

- Deterministic behavior
- Traceability
- Auditability

---

## 9. Logging Requirements

OAP-compliant runtimes SHOULD log:

- Execution ID
- Agent ID
- Trigger type
- Trigger ID
- Timestamp
- Permissions used
- Tools invoked
- Success / Failure status

---

## 10. Future Extensions

Future versions MAY include:

- Cross-agent event propagation
- Webhook standardization
- Real-time streaming triggers
- External event bus integration
- Event replay mechanisms
