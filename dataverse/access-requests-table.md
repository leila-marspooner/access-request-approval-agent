# Access Requests Table

## Purpose

Access Requests is the Dataverse table that stores every request and lifecycle event for the Access Request & Approval Agent.

## Ownership

User or team ownership is recommended so requester, approver, fulfilment/operations, and admin security models can be aligned to RBAC and Least privilege.

## Lifecycle

1. Flow A — Create Request creates a row.
2. Status starts as Pending after validation.
3. Flow B — Approval Watcher sends the Teams Adaptive Card and writes the approver decision.
4. Flow C — Resolution Watcher processes approved/rejected outcomes.
5. Flow D — SLA Reminder / Escalation Watcher updates reminder and escalation fields.
6. GetStatus reads the current lifecycle state from Dataverse.

## Key Governance Controls

- CorrelationId supports traceability.
- ApprovalRequestSent supports approval Idempotency.
- FulfillmentStarted and FulfilledOn support fulfilment Idempotency.
- ReminderCount, LastReminderOn, EscalationCount, EscalatedOn, and EscalatedFlag support SLA tracking.
- ApprovalError and FulfillmentError make operational issues visible.
- Dataverse audit history supports lifecycle review.
