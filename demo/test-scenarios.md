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
| Status lookup | Use a known request with its requester, another user and missing identity | Only the permitted caller receives details; other cases receive a generic denial |
| Missing secure identity | Exercise sign-in, principal-name and email gates separately | Intake stops before Flow A when a required claim is missing |
| Missing manager | Exercise lookup NotFound and a successful lookup with blank manager email separately | No row creation on these paths; capture and verify a safe error response for each |
| SLA item failure | Force one item to fail in a multi-record sweep | Verify the item handler, notification and later-record processing; not established by current captures |

## Evidence Guidance

Configuration, conversation, selected run and model-driven app images are already in the [screenshot index](../screenshots/README.md#evidence-index). The table below distinguishes captured observations from outstanding runtime and mapping checks. Capture additional evidence for those gaps using demo-safe data and redaction; do not treat the scenario matrix as completed test results.

## Captured Evidence and Outstanding Tests

| Scenario | Observed evidence | Remaining verification |
|---|---|---|
| Missing secure identity | [Refusal response](../screenshots/copilotRequestIdentityDenied.png) | Repeat with controlled missing-claim cases |
| Manager lookup succeeds | [Create run](../screenshots/createFlowRunHistorySuccess.png) and [return](../screenshots/createFlowRunSuccessReturn.png) | Expanded manager and Dataverse mappings |
| Manager lookup NotFound | [Error run](../screenshots/createFlowManagerNotFound.png): create branch skipped, CATCH executed | Separately exercise successful lookup with empty manager email |
| Status allowed / denied | [Allowed](../screenshots/copilotStatusAllowed.png) and [denied](../screenshots/copilotStatusOwnershipDenied.png) for AR001025 | Controlled two-user test and server-side predicate inspection |
| Unknown-reference example | [Generic denial](../screenshots/copilotStatusNotFound.png) for AR000000 | Confirm no existence disclosure across all failure paths |
| SLA error scopes | [Configuration](../screenshots/slaWatcherOverview.png) | Run reminder, escalation and item-failure cases; verify delivery and continuation |

These are screenshot observations, not a claim that the entire scenario matrix passed. Simulated fulfilment does not grant real access.
