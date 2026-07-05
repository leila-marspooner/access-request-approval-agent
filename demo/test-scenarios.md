# Test Scenarios

Use demo-safe data only. These scenarios are intended to prove the lifecycle and governance pattern, not production readiness.

## Scenario Matrix

| Scenario | Steps | Expected Result |
|---|---|---|
| Happy path | Submit a valid Standard or Elevated request and approve it in Teams | Status updates, decision fields are populated, Flow C records simulated fulfilment/resolution |
| Admin access gate | Request Admin access | Agent shows policy acknowledgement before Flow A — Create Request is called |
| Cancellation before submission | Stop after the admin acknowledgement prompt | No Access Requests row is created |
| Short justification | Provide a vague justification | Agent asks for a clearer business reason before automation starts |
| Invalid duration | Provide a duration outside the allowed demo range | Agent asks for a corrected duration |
| Rejection path | Submit a valid request and reject through Teams | ApproverDecision, ApproverComments, DecisionOn, Status, and Status Reason are updated; fulfilment is not triggered |
| AI fallback | Simulate or document Azure OpenAI failure | Flow H — SummariseCase returns fallback CaseSummary and request creation can continue |
| Approval idempotency | Retrigger or inspect Flow B logic | ApprovalRequestSent prevents duplicate Teams Adaptive Cards |
| Resolution idempotency | Retrigger or inspect Flow C logic | ResolvedOn and FulfilmentStarted help prevent duplicate resolution work |
| SLA reminder | Use a pending request past the SLA threshold | ReminderCount and LastReminderOn update |
| Escalation | Use a pending request past the reminder limit | Escalated, EscalationCount, and EscalatedOn update |
| Status lookup | Ask GetStatus for a known request | Agent returns the current Dataverse-backed lifecycle state |

## Evidence To Capture

- Copilot Studio topic screenshots
- Dataverse Access Requests row before and after decision
- Teams Adaptive Card approval/rejection
- Flow run evidence for Flow A, Flow B, Flow C, and Flow D
- BYOM fallback evidence where available
- Model-driven app status and SLA views

All evidence must be redacted before public use.
