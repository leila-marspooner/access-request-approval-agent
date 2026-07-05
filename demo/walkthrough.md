# Demo Walkthrough

## Scenario

A requester needs temporary access to a business application. The Access Request Agent collects the request, Flow A — Create Request stores it in Dataverse, Flow B — Approval Watcher routes a Teams Adaptive Card to the approver, and watcher flows handle the outcome and SLA monitoring.

Fulfilment/provisioning is mocked or simulated.

## Step-by-Step Walkthrough

1. Open Access Request Agent (Public) or Access Request Agent (Internal Auth / Secure).
2. Ask to request access.
3. Provide ApplicationName, AccessLevel, Justification, DurationDays, and ApproverEmail.
4. In secure mode, explain that RequestorEmail and RequestorAadObjectId can come from Entra ID context where available.
5. If AccessLevel is Admin, show the policy acknowledgement gate.
6. Submit the request.
7. Show Flow A — Create Request creating the Access Requests row.
8. Show Flow H — SummariseCase returning CaseSummary.
9. Show the Dataverse row with Status, Status Reason, SubmittedOn, CaseSummary, and CorrelationId.
10. Show Flow B — Approval Watcher sending a Teams Adaptive Card.
11. Approve or reject with comments.
12. Show ApproverDecision, ApproverComments, DecisionOn, Status, and Status Reason updated in Dataverse.
13. Ask the agent for status and show GetStatus reading from Dataverse.
14. For an approved request, show Flow C — Resolution Watcher recording simulated fulfilment and ResolvedOn.
15. For a rejected request, show the request closing without simulated fulfilment.
16. Show Flow D — SLA Reminder / Escalation Watcher on an overdue pending request.
17. Show ReminderCount, LastReminderOn, Escalated, EscalationCount, and EscalatedOn.
18. Show the model-driven app views for operational visibility.

## Expected Evidence

| Step | Evidence |
|---|---|
| Request submitted | Access Requests row created |
| AI summary generated | CaseSummary populated or fallback summary returned |
| Approval sent | ApprovalRequestSent and ApprovalRequestSentOn populated |
| Decision captured | ApproverDecision, ApproverComments, and DecisionOn populated |
| Resolution processed | ResolvedOn and relevant fulfilment fields populated |
| SLA monitored | ReminderCount, LastReminderOn, Escalated, EscalationCount, and EscalatedOn updated |

## What Is Mocked

The project does not grant real access. Flow C demonstrates the approval-to-resolution pattern with mock fulfilment notes and lifecycle updates.
