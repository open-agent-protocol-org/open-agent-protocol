# OAP Permission Risk Model (Draft v0.2)

## 1. Purpose

This document defines standardized risk levels for OAP permissions.

The objective is to ensure consistent security enforcement, predictable runtime behavior, and clear user transparency across OAP-compliant environments.

---

## 2. Risk Classification Overview

Each OAP permission MUST be classified into one of the following risk levels:

- Low
- Medium
- High

Runtimes MUST enforce behavior according to the declared risk level.

---

## 3. Low Risk Permissions

Low-risk permissions provide limited or read-only access with minimal potential impact.

### Examples

- calendar.read  
- notifications.send  
- web.browse  
- system.metadata.read  

### Runtime Requirements

- Single confirmation during installation  
- No contextual re-confirmation required  
- Must appear in permission overview  
- Revocation must be supported  

---

## 4. Medium Risk Permissions

Medium-risk permissions allow modification of user data but are generally reversible.

### Examples

- calendar.write  
- files.write  
- email.read  
- profile.update  

### Runtime Requirements

- Explicit confirmation during installation  
- Clear explanation of impact  
- Usage SHOULD be logged  
- User MUST be able to revoke permission  
- Runtime SHOULD allow visibility into permission usage  

---

## 5. High Risk Permissions

High-risk permissions allow sensitive, irreversible, or financial actions.

### Examples

- ecommerce.order  
- email.send  
- system.background_execution  
- financial.transfer  
- account.delete  

### Runtime Requirements

- Explicit confirmation during installation  
- Contextual confirmation at first use  
- Runtime SHOULD allow per-action confirmation  
- Mandatory logging of usage  
- Strong recommendation for additional safeguards (e.g., multi-factor confirmation)

---

## 6. Permission Escalation Rules

When an agent introduces new permissions in an update:

- The runtime MUST re-prompt the user  
- High-risk permissions MUST require explicit opt-in  
- Silent permission upgrades are prohibited  
- Version changes introducing new permissions MUST be logged  

---

## 7. Permission Transparency Guidelines (Non-Normative)

Runtimes SHOULD:

- Display permissions grouped by risk level  
- Visually differentiate risk levels  
- Provide clear human-readable explanations  
- Allow simple permission revocation  
- Allow users to inspect historical usage  

---

## 8. Future Extensions

Future versions MAY include:

- Fine-grained permission scopes  
- Time-limited permissions  
- Transaction-limited permissions  
- Delegated permissions  
- Capability-based access control  
