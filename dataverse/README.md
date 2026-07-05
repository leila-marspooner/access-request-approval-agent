# Dataverse Design

Dataverse is the system of record for the **Access Request & Approval Agent**.

The Access Requests table stores request intake, identity, lifecycle state, approval decisions, AI summary, SLA monitoring fields, error capture, operational metrics, and audit evidence. Power Automate watcher flows read from and write back to Dataverse so the model-driven app can show the request lifecycle without relying on chat history or flow run history alone.

## Main Table

- **Access Requests**

## Why Dataverse

Dataverse is used because the project needs:

- Structured request records
- Status and Status Reason lifecycle tracking
- Role-based access control
- Field security options for internal fields
- Audit history for key lifecycle changes
- Model-driven app visibility
- Power Automate trigger support
- ALM-friendly solution packaging

## Column Groups

| Group | Example Fields |
|---|---|
| Request identity | Request Number, RequestName, Request Title (Primary), CorrelationId |
| Requester and approver | RequestorEmail, RequestorAadObjectId, ApproverEmail |
| Request details | ApplicationName, AccessLevel, Justification, DurationDays |
| Lifecycle | Status, Status Reason, SubmittedOn, DecisionOn, ResolvedOn |
| Approval | ApprovalRequestSent, ApprovalRequestSentOn, ApproverDecision, ApproverComments, ApprovalError |
| AI summary | CaseSummary |
| Fulfilment/resolution | FulfilmentStarted, FulfilmentNotes, FulfilmentError |
| SLA/escalation | ReminderCount, LastReminderOn, Escalated, EscalationCount, EscalatedOn |
| Metrics | TimeToApproveHours, TimeToResolutionHours, PostDecisionResolutionHours |

## Model-driven App Role

The model-driven app gives reviewers and operators a structured way to inspect the lifecycle:

- Pending and approved requests
- Rejected requests
- Overdue or escalated requests
- Records with approval or fulfilment errors
- Audit history and operational traceability

## Security Notes

RBAC should separate requester, approver, fulfilment/operations, admin, and optional automation service responsibilities.

Only claim security roles, Field Security Profile configuration, and audit settings as implemented where public-safe evidence exists. Otherwise, describe them as documented governance controls or hardening recommendations.

## Related Files

- [Access Requests table](access-requests-table.md)
- [Columns](columns.md)
- [Choices](choices.md)
- [Security roles](security-roles.md)
- [Model-driven app](model-driven-app.md)
