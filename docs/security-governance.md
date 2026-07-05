# Security and Governance — Access Request & Approval Agent

## Overview

This document explains the security, governance, auditability, and operational control patterns demonstrated in the **Access Request & Approval Agent**.

The project is designed as a Power Platform / Copilot Studio portfolio case study. Its purpose is to show how a conversational agent can be connected to a governed business process, using Dataverse, Power Automate, Teams, Entra ID, Azure OpenAI BYOM summarisation, RBAC, audit history, error capture, and ALM discipline.

The main security message is:

> Copilot Studio provides the user experience, but Power Automate and Dataverse control the business process.

This separation is important because approval workflows need traceability, deterministic processing, human review, role-based access, idempotency, and supportable error handling.

---

## What This Demonstrates to Employers

This project demonstrates practical governance patterns that are relevant to Power Platform, Copilot Studio, and Microsoft 365 roles.

It shows experience with:

- Designing a secure agent-backed business process
- Using Dataverse as the system of record
- Separating chat experience from approval authority
- Applying role-based access control
- Capturing signed-in user identity in a secure variant
- Building human-in-the-loop approval with Teams Adaptive Cards
- Helping prevent duplicate approval and fulfilment processing
- Capturing operational errors back to Dataverse
- Using audit history for traceability
- Managing configuration with environment variables and connection references
- Treating AI output as decision support, not automated authority
- Thinking about ALM, release readiness, and public repo safety

This is the type of governance thinking expected in enterprise Power Platform projects.

---

## Scope and Disclaimer

This is a **portfolio/demo project**, not a live production access management system.

It does not replace:

- Microsoft Entra ID Governance
- A production identity governance platform
- An enterprise ITSM/access management platform
- A real provisioning workflow

Fulfilment/provisioning is mocked or simulated unless future evidence shows a real integration.

A real production deployment would require additional security review, DLP policy validation, environment hardening, monitoring, support processes, and integration with approved provisioning systems.

---

## Governance Design Principles

The project follows these design principles:

| Principle | How It Is Demonstrated |
|---|---|
| Dataverse as system of record | Request state, decisions, timestamps, errors, reminders, escalation, and metrics are stored in Dataverse |
| Human approval remains central | The AI summary supports the approver, but the approver makes the decision |
| Chat does not own the process | Copilot Studio collects data; Power Automate and Dataverse control lifecycle state |
| Least privilege | Roles separate requester, approver, fulfilment/operations, and admin responsibilities |
| Idempotency by design | Flags and timestamps help prevent duplicate Teams cards, duplicate fulfilment, and uncontrolled escalation |
| Auditability | Key lifecycle fields can be audited through Dataverse audit history |
| Supportability | Errors are written back to Dataverse rather than hidden only in flow run history |
| Configurable deployment | Environment variables and connection references support safer ALM patterns |
| Safe AI use | BYOM summarisation is optional support, not an access decision-maker |

---

## Authentication Modes

The project supports two intentional authentication modes.

| Mode | Purpose | Identity Pattern |
|---|---|---|
| Access Request Agent (Public) | Low-friction portfolio review and demo testing | Requester email can be collected conversationally |
| Access Request Agent (Internal Auth / Secure) | Enterprise-aligned secure mode | RequestorEmail / UPN and RequestorAadObjectId are captured from signed-in Entra ID context |

### Public / Demo Mode

The public/demo variant is designed to make the project easier for reviewers to understand without needing tenant credentials.

In this mode:

- No sign-in is required.
- Requester email may be captured conversationally.
- The experience is suitable for portfolio review and demo walkthroughs.
- It should not be presented as a secure production pattern.

### Secure / Enterprise Mode

The secure variant demonstrates Entra ID-backed identity capture.

In this mode:

- Users are required to sign in.
- RequestorEmail / UPN is captured from authenticated context where available.
- RequestorAadObjectId can be stored in Dataverse for stronger identity binding.
- The request is associated with a signed-in identity rather than only user-typed data.

This shows how the public demo pattern can be adapted toward an enterprise identity-aware design.

---

## Identity Fields

The Access Requests table includes identity-related fields to support traceability.

| Field | Purpose |
|---|---|
| RequestorEmail | Stores the requester email or UPN |
| RequestorAadObjectId | Stores the requester Entra ID object ID where captured |
| ApproverEmail | Stores the designated approver email |
| Owner | Standard Dataverse ownership field |
| Created By | Standard Dataverse creator field |
| Modified By | Standard Dataverse modifier field |

In the secure variant, the preferred pattern is to populate requester identity from Entra ID rather than relying on user-entered values.

---

## RBAC and Least Privilege

Role-Based Access Control is used to separate responsibilities across the request lifecycle.

Recommended roles:

| Role | Intent |
|---|---|
| Access Request - Requester | Create requests and read own records |
| Access Request - Approver | Read assigned or pending requests and update approval decision fields |
| Access Request - Fulfilment/Ops | Read/write operational, resolution, and fulfilment fields |
| Access Request - Admin | Manage the model-driven app, request records, configuration, and governance evidence |
| Access Request - Automation Service | Optional service role for flows and automation |

### Requester Role

The requester should only have the privileges needed to submit and view their own requests.

Recommended pattern:

- Create Access Request records
- Read own Access Request records
- No delete
- No assign
- No share
- No access to internal/system fields unless explicitly required

### Approver Role

The approver needs access to review pending requests and record decisions.

Recommended pattern:

- Read relevant Access Request records
- Update approval decision fields
- No delete
- No unrestricted admin privileges
- No access to sensitive system configuration

Key fields for approvers may include:

- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail
- CaseSummary
- ApproverDecision
- ApproverComments
- DecisionOn
- Status
- Status Reason

### Fulfilment / Operations Role

The fulfilment or operations role supports lifecycle closure and operational investigation.

Recommended pattern:

- Read request records
- Update fulfilment/resolution fields
- Review error fields
- Review reminder and escalation state
- No unnecessary admin-level privileges

Key fields may include:

- Status
- Status Reason
- ResolvedOn
- FulfilmentStarted
- FulfilmentNotes
- FulfilmentError
- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn
- TimeToResolutionHours
- PostDecisionResolutionHours

### Admin Role

The admin role supports full operational oversight and governance review.

Recommended pattern:

- Manage Access Request records
- Review audit history
- Review environment variables and connection references
- Manage model-driven app configuration
- Review RBAC and Field Security Profile evidence
- Support release readiness and ALM evidence

### Automation Service Role

An optional automation service role can be used for cloud flows and service identities.

Recommended pattern:

- Least-privilege service-level access
- Read/write only to required Dataverse fields
- No delete unless explicitly required
- No personal maker connection dependency in UAT/production-style environments

Public documentation should only describe RBAC as implemented where screenshots or evidence exist. Otherwise, describe it as a documented governance control or hardening pattern.

---

## Field Security Profile

Field-level security can protect sensitive or internal/system fields.

Recommended secured fields include:

| Field | Why It May Need Protection |
|---|---|
| RequestorAadObjectId | Internal identity binding value |
| CorrelationId | Internal traceability identifier |
| CaseSummary | AI-generated summary that may include operational context |
| ApprovalError | Internal troubleshooting detail |
| FulfilmentError | Internal troubleshooting detail |
| FulfilmentNotes | Internal fulfilment or operational notes |
| Escalated | Operational escalation flag |
| EscalationCount | Operational escalation tracking |
| EscalatedOn | Operational escalation timestamp |

Only claim Field Security Profile implementation where evidence exists.

If screenshots or configuration evidence are not yet available, describe field-level security as:

> A documented governance control and recommended hardening item.

---

## Draft-Only Editing Guardrail

Draft-only editing protects the integrity of submitted requests.

Recommended rule:

> If Status is no longer Draft, requester-controlled fields become read-only.

Fields to lock after submission may include:

- Request Title (Primary)
- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail where appropriate

This prevents users from changing request details after the process has started or after the approver has reviewed the request.

This is important for auditability because the approval decision should be based on the original submitted request.

---

## Dataverse Audit History

Dataverse audit history supports traceability for key lifecycle changes.

Recommended audited fields include:

### Lifecycle Fields

- Status
- Status Reason
- SubmittedOn
- DecisionOn
- ResolvedOn

### Approval Fields

- ApprovalRequestSent
- ApprovalRequestSentOn
- ApproverDecision
- ApproverComments
- ApprovalError

### Fulfilment / Resolution Fields

- FulfilmentStarted
- FulfilmentNotes
- FulfilmentError

### SLA / Escalation Fields

- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn

### Reporting Metrics

- TimeToApproveHours
- TimeToResolutionHours
- PostDecisionResolutionHours

Screenshots of audit history must be redacted before publishing.

Do not publish audit screenshots that expose real user names, real email addresses, tenant URLs, environment details, or live business data.

---

## Idempotency Controls

idempotency prevents duplicate processing when Dataverse triggers fire more than once or flows are retried.

### Approval Idempotency

Approval idempotency is controlled through:

- ApprovalRequestSent
- ApprovalRequestSentOn

Recommended pattern:

1. Flow B — Approval Watcher detects a pending request.
2. The flow checks whether ApprovalRequestSent is already true.
3. If ApprovalRequestSent is false, the flow stamps ApprovalRequestSent and ApprovalRequestSentOn.
4. The flow then sends the Teams Adaptive Card.
5. If the watcher retriggers, the flow exits without sending another card.

This prevents duplicate Teams Adaptive Cards.

### Fulfilment / Resolution Idempotency

Fulfilment and resolution idempotency can be controlled through:

- FulfilmentStarted
- ResolvedOn
- Status
- Status Reason

Recommended pattern:

1. Flow C — Resolution Watcher detects an approved or rejected outcome.
2. The flow checks whether the request has already been resolved.
3. If not resolved, it processes the outcome.
4. The flow stamps ResolvedOn and any fulfilment-related fields.
5. If the watcher retriggers, it exits without duplicating resolution or fulfilment work.

This prevents duplicate simulated fulfilment or repeated requester notifications.

### SLA Reminder Idempotency

SLA reminder behaviour is controlled through:

- ReminderCount
- LastReminderOn

Recommended pattern:

1. Flow D — SLA Reminder / Escalation Watcher finds overdue pending approvals.
2. It checks the current reminder count and last reminder timestamp.
3. It sends a reminder only when the request meets the configured threshold.
4. It increments ReminderCount and stamps LastReminderOn.

This reduces reminder spam and makes reminder activity auditable.

### Escalation Idempotency

Escalation behaviour is controlled through:

- Escalated
- EscalationCount
- EscalatedOn

Recommended pattern:

1. Flow D checks whether the request has exceeded the configured reminder threshold.
2. If escalation is required, it sets Escalated.
3. It increments EscalationCount and stamps EscalatedOn.
4. It avoids repeatedly escalating the same request without control.

There is no separate risk flag field in the current schema. Escalation is tracked through Escalated, EscalationCount, and EscalatedOn.

---

## Error Capture

The project uses TRY/CATCH-style flow handling so operational errors are written back to Dataverse.

The goal is to make failures visible from the model-driven app without requiring every reviewer or support user to inspect Power Automate run history.

Recommended error fields:

| Field | Purpose |
|---|---|
| ApprovalError | Stores concise approval watcher error detail |
| FulfilmentError | Stores concise resolution or simulated fulfilment error detail |

### Approval Errors

Flow B — Approval Watcher should write errors to ApprovalError when approval card posting or decision capture fails.

Examples:

- Teams card could not be posted
- Approver destination invalid
- Adaptive Card response failed
- Dataverse update failed

### Resolution / Fulfilment Errors

Flow C — Resolution Watcher should write errors to FulfilmentError when resolution or simulated fulfilment fails.

Examples:

- Requester notification failed
- Fulfilment simulation failed
- Dataverse update failed
- Flow retry exhausted

### SLA Watcher Errors

Flow D — SLA Reminder / Escalation Watcher should handle item-level failures where possible so one problematic request does not break the entire scheduled sweep.

Where implemented, errors should be written back to the relevant request row or captured in a separate operational log.

---

## Correlation and Traceability

CorrelationId is generated when the request is created.

It supports tracing across:

- Copilot Studio submission
- Flow A — Create Request
- Flow H — SummariseCase
- Dataverse Access Requests row
- Flow B — Approval Watcher
- Teams Adaptive Card
- Flow C — Resolution Watcher
- Flow D — SLA Reminder / Escalation Watcher
- Model-driven app investigation
- Power Automate run history

Recommended practice:

- Generate CorrelationId at request creation.
- Store it on the Dataverse row.
- Include it in relevant operational messages where safe.
- Use it when investigating failed or disputed requests.

---

## AI Governance

Flow H — SummariseCase uses Azure OpenAI BYOM to generate CaseSummary.

The AI summary supports the approver but does not make access decisions.

### AI Design Principles

- AI is used for reviewer support, not automated access approval.
- The approver remains the human decision-maker.
- If the AI call fails, the parent request process should continue.
- A fallback summary should be returned so the request is not blocked.
- The summary should be stored in Dataverse as part of the request record.
- The summary should be treated as operational context, not ground truth.

### BYOM Configuration

BYOM configuration should be externalised through environment variables.

Recommended configuration values include:

- BYOM endpoint
- BYOM deployment name
- BYOM API version
- BYOM system prompt
- SLA reminder threshold
- Maximum reminder count
- Escalation recipient
- Approval routing destination

Secrets and sensitive configuration should use Key Vault-backed patterns where implemented.

Do not hardcode or publish:

- API keys
- Client secrets
- Bearer tokens
- Private endpoints
- Tenant-specific URLs
- Prompts containing sensitive business data
- Private connection details

---

## Environment Variables and Configuration

Environment variables support ALM and safer deployment.

They allow environment-specific values to be changed without editing flow logic.

Recommended configuration areas:

| Configuration Area | Example |
|---|---|
| BYOM endpoint | Azure OpenAI endpoint |
| BYOM deployment name | Model deployment name |
| BYOM API version | API version string |
| BYOM system prompt | Summary prompt configuration |
| SLA threshold | Reminder timing |
| Maximum reminders | Escalation threshold |
| Escalation recipient | Escalation mailbox or Teams recipient |
| Approval routing destination | Approver mailbox, Teams user, or channel |

No endpoint, key, channel ID, recipient address, or tenant-specific value should be hardcoded in flow actions if it can be externalised.

---

## Connection References

Connection references should be used for connectors so flows are not tied directly to a personal maker connection.

Recommended connectors may include:

- Dataverse
- Microsoft Teams
- Office 365 Outlook where used
- Azure OpenAI / HTTP connector where used
- Key Vault where used

For UAT or production-style deployments, connection references should be bound by the importing admin or service account rather than relying on a personal maker account.

---

## ALM and Release Governance

The project demonstrates ALM discipline through solution packaging and release readiness checks.

Recommended solution identity:

`lai_AccessRequestAgent`

Recommended ALM principles:

- Use unmanaged solution in Dev.
- Export managed solution for UAT/production-style deployment.
- Use environment variables for environment-specific configuration.
- Use connection references for connectors.
- Do not hardcode endpoints, keys, tenant values, channel IDs, or recipient addresses.
- Use MAJOR.MINOR.PATCH.BUILD versioning.
- Retain previous managed solution packages as rollback artefacts.
- Do not make direct hotfixes in UAT or production-style environments.
- Fix in Dev, version, export, and redeploy.

### Release Evidence

A release evidence pack should include:

- Managed solution export evidence
- Environment variable configuration screenshot
- Connection reference screenshot
- RBAC role evidence
- Field Security Profile evidence where implemented
- Audit history screenshot
- Flow run history for happy path
- Flow run history for error/fallback path
- Idempotency proof
- BYOM fallback proof
- SLA watcher run evidence
- Screenshot redaction check

---

## Model-Driven App Governance View

The model-driven app should support operational and governance review.

Useful form sections include:

### Request Details

- Request Number
- Request Title
- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail
- RequestorAadObjectId
- ApproverEmail

### Approval

- Status
- Status Reason
- ApprovalRequestSent
- ApprovalRequestSentOn
- ApproverDecision
- ApproverComments
- DecisionOn
- ApprovalError

### Resolution / Fulfilment

- FulfilmentStarted
- ResolvedOn
- FulfilmentNotes
- FulfilmentError

### Summary and Traceability

- CaseSummary
- CorrelationId

### SLA / Operations

- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn

### Metrics

- TimeToApproveHours
- TimeToResolutionHours
- PostDecisionResolutionHours

### Audit

- Audit history
- Created By
- Created On
- Modified By
- Modified On
- Owner

---

## Public Safety Rules

Never publish:

- Secrets
- API keys
- Client secrets
- Bearer tokens
- Passwords
- Tenant IDs
- Tenant URLs
- Environment URLs
- Endpoint URLs
- Private environment names
- Real email addresses
- Private approver names
- Personal data
- Live Dataverse data
- Connection details
- Connection secrets
- Private flow run data
- Azure OpenAI endpoint values
- Key Vault secret values
- Screenshots exposing sensitive configuration

Use demo data and redacted screenshots only.

---

## Screenshot Redaction Checklist

Before publishing screenshots, check for:

- Email addresses
- Tenant names
- Tenant IDs
- Environment names
- URLs
- Client IDs
- Secrets
- Tokens
- API keys
- Connection names
- Personal maker accounts
- Real user records
- Browser tabs and bookmarks
- Live Dataverse data
- Private Teams channel names
- Power Automate run inputs or outputs

If in doubt, redact.

---

## What Can Be Claimed Publicly

Safe public claims:

- This is a portfolio/demo case study.
- The architecture demonstrates enterprise-aligned governance patterns.
- Dataverse is used as the system of record.
- Power Automate is used for deterministic lifecycle orchestration.
- Teams Adaptive Cards provide human-in-the-loop approval.
- BYOM summarisation supports reviewers but does not make decisions.
- Public and secure agent variants demonstrate different deployment contexts.
- Fulfilment is mocked or simulated.
- Production deployment would require further hardening and review.

Avoid claiming:

- This is production-ready.
- It replaces Entra ID Governance.
- It provisions real access unless real evidence exists.
- Field Security Profile is fully implemented unless screenshots prove it.
- DLP policies are fully configured unless evidence exists.
- It has been deployed to real production users unless true.

---

## Summary

The Access Request & Approval Agent demonstrates governance-first design for a Copilot Studio and Power Platform workflow.

Its security value comes from the combination of:

- Entra ID secure identity pattern
- Dataverse as system of record
- RBAC and least privilege
- Draft-only editing guardrails
- Field-level security where implemented
- Dataverse audit history
- Idempotency fields
- CorrelationId tracing
- Error capture back to the record
- Human-in-the-loop approval
- AI used only as reviewer support
- Environment-variable-driven configuration
- Connection references and ALM discipline

The project should remain clearly positioned as a portfolio/demo case study unless it is later hardened, reviewed, monitored, and integrated with real production provisioning systems.
