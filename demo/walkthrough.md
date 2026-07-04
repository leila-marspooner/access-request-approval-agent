# Demo Walkthrough

## Scenario

A requester needs temporary access to a business application. The Access Request Agent collects the request, Flow A — Create Request stores it in Dataverse, Flow B — Approval Watcher routes a Teams Adaptive Card to the approver, and watcher flows handle the outcome and SLA monitoring.

## Walkthrough

1. Open the Access Request Agent.
2. Ask to request access.
3. Provide application, AccessLevel, Justification, DurationDays, and ApproverEmail.
4. If AccessLevel is Admin, acknowledge the policy gate.
5. Submit the request.
6. Show the returned request reference, Status, CaseSummary, and CorrelationId.
7. Open Dataverse Access Requests and show the new row.
8. Show Flow B — Approval Watcher sending a Teams Adaptive Card.
9. Approve or reject with comments.
10. Show ApproverDecision, ApproverComments, DecisionOn, and Status updated in Dataverse.
11. Show GetStatus returning the current lifecycle state.
12. Show Flow C — Resolution Watcher performing simulated fulfilment for an approved request.
13. Show Flow D — SLA Reminder / Escalation Watcher fields on a pending or overdue request.
14. Show Model-driven app views for operational visibility.

## Reminder

Fulfilment/provisioning is mocked unless real provisioning evidence is added later.
