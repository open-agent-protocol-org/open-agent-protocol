OAP Event & Trigger Model (Draft v0.2)
1. Purpose

This document defines how OAP-compliant agents are triggered and executed.

The objective is to standardize agent activation behavior across runtimes.

2. Trigger Types

An OAP agent MAY declare one or more of the following trigger types:

Manual

Scheduled

Event-Based

If no trigger is declared, manual invocation is assumed.

3. Manual Trigger

Manual triggers allow users to explicitly invoke the agent.

Manifest example:

{
  "triggers": {
    "manual": true
  }
}
Runtime Requirements

Must allow explicit user invocation

Must log manual executions

Must inject execution metadata

4. Scheduled Trigger

Scheduled triggers allow agents to execute on a defined schedule.

Manifest example:

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
Runtime Requirements

Must allow users to modify schedules

Must allow disabling schedules

Must log scheduled executions

Must inject trigger metadata

5. Event-Based Trigger

Event triggers allow agents to react to system or external events.

Manifest example:

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
Required Fields

Each event trigger MUST define:

id

source

event_type

Optional fields:

filter

debounce

Runtime Responsibilities

Validate required permissions

Allow trigger enable/disable

Log event-driven executions

Enforce permission boundaries

6. Background Execution Permission

Agents declaring scheduled or event triggers implicitly require:

"system.background_execution"

Runtimes SHOULD require explicit user consent for background execution.

7. Execution Metadata Injection

During execution, the runtime MUST inject metadata:

{
  "execution_context": {
    "trigger_type": "manual | scheduled | event",
    "trigger_id": "<trigger_id>",
    "timestamp": "<ISO8601 timestamp>"
  }
}

This ensures traceability and auditability.

8. Logging Requirements

OAP-compliant runtimes SHOULD log:

Execution ID

Agent ID

Trigger type

Trigger ID

Timestamp

Permissions used

Tools invoked

Success / Failure status

9. Future Extensions

Future versions MAY include:

Cross-agent event propagation

Webhook standardization

Event replay mechanisms

External event bus integration
