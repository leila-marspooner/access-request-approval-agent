# Access Requests Table

## Purpose

**Access Requests** is the Dataverse table that holds the lifecycle record for each request.

It is the authoritative source for request details, status, decisions, timestamps, AI summary, reminders, escalation, errors, and operational metrics.

## Lifecycle Pattern

1. Flow A — Create Request creates the Access Requests row.
2. Flow H — SummariseCase returns CaseSummary for reviewer support.
3. Flow B — Approval Watcher sends one Teams Adaptive Card and writes the decision back.
4. Flow C — Resolution Watcher processes the approved or rejected outcome and records simulated fulfilment where appropriate.
5. Flow D — SLA Reminder / Escalation Watcher monitors overdue pending approvals.
6. GetStatus reads the current lifecycle state from Dataverse for Copilot Studio.

## Ownership

User or team ownership is recommended so requester, approver, fulfilment/operations, admin, and optional automation service roles can be aligned to least privilege.

## Key Governance Controls

| Control | Supporting Fields |
|---|---|
| Traceability | Request Number, CorrelationId |
| Approval idempotency | ApprovalRequestSent, ApprovalRequestSentOn |
| Resolution/fulfilment idempotency | FulfilmentStarted, ResolvedOn, Status, Status Reason |
| SLA monitoring | ReminderCount, LastReminderOn |
| Escalation tracking | Escalated, EscalationCount, EscalatedOn |
| Error capture | ApprovalError, FulfilmentError |
| Operational reporting | TimeToApproveHours, TimeToResolutionHours, PostDecisionResolutionHours |

## Portfolio Scope

The table demonstrates a governed lifecycle pattern. It does not prove production deployment, and fulfilment/provisioning remains mocked or simulated unless future evidence shows a real provisioning integration.
