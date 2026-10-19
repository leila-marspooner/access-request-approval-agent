# Demo Walkthrough

## Scenario

A requester needs temporary access to a business application. The Access Request Agent collects the request, Flow A — Create Request stores it in Dataverse, Flow B — Approval Watcher routes a Teams Adaptive Card to the approver, and watcher flows handle the outcome and SLA monitoring.

Fulfilment/provisioning is mocked or simulated.

## Step-by-Step Walkthrough

1. Open Access Request Agent (Public) or Access Request Agent (Internal Auth / Secure).
2. Ask to request access.
3. In secure mode, show the sign-in/principal-name/email gates before intake. Public/demo mode uses typed requester and approver emails.
4. Provide ApplicationName, AccessLevel, Justification and DurationDays. In secure mode explain signed-in email and the principal-name mapping into the existing ApproverEmail flow input; Flow A contains manager lookup. Object-ID capture is not shown in this topic call.
5. If AccessLevel is Admin, show the policy acknowledgement gate.
6. Submit the request.
7. Show Flow A manager lookup and row creation in the secure path. Compare the successful pictured run with lookup NotFound/CATCH evidence; do not claim the blank-manager-email branch was tested.
8. Show Flow H — SummariseCase returning CaseSummary.
9. Show the Dataverse row with Status, Status Reason, SubmittedOn, CaseSummary, and CorrelationId.
10. Show Flow B — Approval Watcher sending a Teams Adaptive Card.
11. Approve or reject with comments.
12. Show ApproverDecision, ApproverComments, DecisionOn, Status, and Status Reason updated in Dataverse.
13. Show the allowed and generic denied GetStatus responses. Explain that identities and the server-side ownership predicate still need controlled verification.
14. For an approved request, show Flow C — Resolution Watcher recording simulated fulfilment and ResolvedOn.
15. For a rejected request, show the request closing without simulated fulfilment.
16. Show Flow D reminder/escalation and per-request/outer error-scope configuration.
17. Explain the intended ReminderCount, LastReminderOn, Escalated, EscalationCount and EscalatedOn updates. Current captures do not verify exact updates, message delivery or continuation after item failure.
18. Show the model-driven app views for operational visibility.

## Evidence to Verify During a Live Walkthrough

This is a walkthrough checklist, not a record of passing tests. See [captured observations and outstanding tests](test-scenarios.md#captured-evidence-and-outstanding-tests).

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
