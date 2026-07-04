# Test Scenarios

Use demo-safe data only.

## Happy Path

- Standard access request.
- Valid justification.
- Valid duration.
- Approver approves in Teams.
- Flow C — Resolution Watcher marks the request Fulfilled with FulfillmentNotes.

## Admin Access Gate

- Request Admin access.
- Confirm the policy acknowledgement appears.
- Continue and confirm Flow A — Create Request runs only after acknowledgement.

## Cancellation Before Submission

- Request Admin access.
- Do not proceed after the policy acknowledgement.
- Confirm no Access Requests row is created.

## Short Justification

- Provide a vague or too-short justification.
- Confirm the agent asks for a clearer business reason before automation starts.

## Invalid Duration

- Provide a duration outside the allowed range.
- Confirm the agent asks for a corrected value.

## Rejection Path

- Submit a valid request.
- Reject through the Teams Adaptive Card.
- Confirm ApproverDecision, ApproverComments, DecisionOn, and Status are updated.
- Confirm Flow C — Resolution Watcher does not perform fulfilment.

## AI Fallback

- Simulate or document Azure OpenAI failure.
- Confirm Flow H — SummariseCase returns a fallback CaseSummary.
- Confirm Flow A — Create Request can continue.

## Approval Idempotency

- Confirm ApprovalRequestSent prevents duplicate Teams Adaptive Cards.
- Confirm ApprovalRequestSentOn is stamped.

## SLA Reminder

- Use a pending request past the SLA threshold.
- Confirm ReminderCount and LastReminderOn update.

## Escalation

- Use a pending request past the reminder limit.
- Confirm EscalationCount, EscalatedOn, and EscalatedFlag update.
