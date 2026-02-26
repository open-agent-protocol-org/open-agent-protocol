OAP Event & Trigger Model (v0.2 Draft)

This document defines how OAP agents are triggered and executed.

Agents should not be limited to manual invocation.
They must support structured trigger models.

Trigger Categories

OAP defines three primary trigger types:

Manual

Scheduled

Event-based

1. Manual Trigger

The agent runs when explicitly invoked by a user.

Example:

"triggers": {
  "manual": true
}

Manual triggers are required for all agents unless explicitly designed as background-only agents.

Runtime requirements:

Must allow explicit user invocation

Must log manual executions

Must inject execution metadata

2. Scheduled Trigger

Agents may declare scheduled executions using cron syntax.

Example:

"triggers": {
  "scheduled": [
    {
      "id": "daily_summary",
      "cron": "0 7 * * *",
      "description": "Runs every day at 07:00"
    }
  ]
}

Runtime Requirements:

Must allow user modification of schedule

Must allow disabling scheduled triggers

Must log scheduled executions

Must inject trigger metadata into execution context

3. Event-Based Trigger

Agents may respond to external system events.

Example:

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

Runtime Responsibilities:

Validate permissions required for event source

Allow users to enable/disable specific event triggers

Log event-driven executions

Enforce permission boundaries

Event Object Structure

Each event trigger must include:

id (string, required)

source (string, required)

event_type (string, required)

filter (object, optional)

debounce (object, optional)

Example structure:

{
  "id": "new_email",
  "source": "email",
  "event_type": "message.received",
  "filter": {
    "from_domain": "company.com"
  },
  "debounce": {
    "window_seconds": 30
  }
}
Background Execution

If an agent declares scheduled or event triggers, it implicitly requests:

"permissions": ["system.background_execution"]

High-risk background agents must require explicit user consent.

Trigger Metadata Injection

At execution time, the runtime must inject metadata into the agent context.

Example:

{
  "execution_context": {
    "trigger_type": "scheduled",
    "trigger_id": "daily_summary",
    "timestamp": "2026-02-26T07:00:00Z"
  }
}

This ensures:

Deterministic execution

Auditability

Runtime traceability

Logging Requirements

OAP-compliant runtimes should log:

Execution ID

Agent ID

Trigger type

Trigger ID

Timestamp

Permissions used

Tools invoked

Success / Failure status

Future Considerations (Post v0.2)

Cross-agent event propagation

Webhook standardization

Real-time streaming triggers

External event bus integration

Event replay and recovery mechanisms
