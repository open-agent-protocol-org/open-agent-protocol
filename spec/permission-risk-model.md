OAP Permission Risk Model (Draft v0.2)
1. Purpose

This document defines standardized risk levels for OAP permissions.

The objective is to ensure consistent security enforcement and user transparency across runtimes.

2. Risk Levels

OAP defines three permission risk levels:

Low

Medium

High

Each declared permission MUST be classified into one of these categories.

3. Low Risk Permissions

Low-risk permissions provide read-only or non-sensitive access.

Examples:

calendar.read

notifications.send

web.browse (read-only)

system.metadata.read

Runtime Requirements:

Single confirmation at installation

No re-confirmation required unless scope changes

Must be visible in permission overview

4. Medium Risk Permissions

Medium-risk permissions allow modification of user data but are reversible.

Examples:

calendar.write

files.write

email.read

profile.update

Runtime Requirements:

Explicit confirmation at installation

Must be displayed clearly with description

Runtime SHOULD log each usage instance

User MUST be able to revoke permission

5. High Risk Permissions

High-risk permissions allow irreversible, sensitive, or financial actions.

Examples:

ecommerce.order

email.send

system.background_execution

financial.transfer

account.delete

Runtime Requirements:

Explicit confirmation at installation

Contextual confirmation at first usage

Optional per-action confirmation

Mandatory detailed logging

Strong recommendation for multi-factor confirmation

6. Permission Escalation Rules

If an agent updates and introduces new permissions:

Runtime MUST re-prompt user for approval

High-risk additions MUST require explicit opt-in

Silent permission upgrades are prohibited

7. UI Guidelines (Non-Normative)

Runtimes SHOULD:

Display permissions grouped by risk level

Use visual indicators (color coding or badges)

Clearly explain why each permission is required

Allow easy permission revocation

8. Future Extensions

Future versions MAY include:

Fine-grained permission scopes

Time-bound permissions

Transaction-limited permissions

Delegated permission models
