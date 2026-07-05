# Power Automate Flows

The **Access Request & Approval Agent** uses a six-flow Power Automate architecture.

The split is deliberate: each flow has a clear responsibility and is designed so it can be tested, evidenced, retried, and monitored separately.

Copilot Studio provides the conversational front end, but Power Automate controls the deterministic business process: request creation, AI summarisation, approval routing, resolution, status lookup, SLA reminders, escalation, and error capture.

---

## Why Power Automate?

Approval, resolution, SLA monitoring, and fulfilment logic are implemented in **Power Automate** rather than Agent Flows because these lifecycle steps need:

- Deterministic branching
- Retry handling
- Run history
- TRY/CATCH-style error handling
- Idempotency controls
- Dataverse write-back
- Supportability outside the chat layer
- Clear evidence for audit and governance review

This design keeps Copilot Studio focused on user interaction while Power Automate and Dataverse control the business lifecycle.

---

## Flow Summary

| Flow | Trigger | Role |
|---|---|---|
| Flow A — Create Request | Copilot Studio | Validates request input, generates CorrelationId, calls SummariseCase, creates the Dataverse Access Requests record, and returns structured confirmation to Copilot Studio |
| Flow H — SummariseCase | Child flow called by Flow A | Generates CaseSummary through Azure OpenAI BYOM using environment-variable-driven configuration and fallback handling |
| GetStatus | Copilot Studio | Returns the current request lifecycle state from Dataverse to Copilot Studio |
| Flow B — Approval Watcher | Dataverse row added or modified | Sends one Teams Adaptive Card approval request and captures the approver decision back into Dataverse |
| Flow C — Resolution Watcher | Dataverse row modified | Processes approved/rejected outcomes, stamps resolution fields, and performs simulated fulfilment where appropriate |
| Flow D — SLA Reminder / Escalation Watcher | Scheduled recurrence | Monitors overdue approvals, sends reminders, increments reminder counters, and records escalation state |

---

## Flow A — Create Request

### Purpose

Flow A is called by Copilot Studio when the requester submits an access request.

It creates the main Dataverse Access Requests row and starts the lifecycle.

### Trigger

Copilot Studio calls the flow.

### Main Responsibilities

- Receive structured request details from Copilot Studio
- Validate required inputs
- Generate a CorrelationId
- Call Flow H — SummariseCase
- Create a Dataverse Access Requests row
- Set the initial lifecycle state
- Return request confirmation to Copilot Studio

### Typical Inputs

- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail
- RequestorAadObjectId where available
- ApproverEmail

### Key Dataverse Fields Updated

- Request Number
- RequestName
- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail
- RequestorAadObjectId
- ApproverEmail
- Status
- Status Reason
- SubmittedOn
- CaseSummary
- CorrelationId

### Outputs to Copilot Studio

- Request identifier / Request Number
- Status
- CaseSummary where appropriate
- CorrelationId where appropriate
- Success or failure response

### Governance Notes

- CorrelationId is generated at request creation for traceability.
- Request creation should fail safely if required fields are missing.
- AI summarisation should not block the whole process if the model call fails.
- Error responses should be user-friendly and not expose technical details.

---

## Flow H — SummariseCase

### Purpose

Flow H is a reusable child flow that generates a short CaseSummary to help the approver review the request.

The AI summary supports the human reviewer. It does not approve, reject, or provision access.

### Trigger

Called by Flow A — Create Request.

### Main Responsibilities

- Receive request details
- Build a compact prompt for summarisation
- Call Azure OpenAI using BYOM configuration
- Return CaseSummary to Flow A
- Return a safe fallback summary if the AI call fails

### Typical Inputs

- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail

### Typical Output

- CaseSummary

### Configuration Pattern

BYOM configuration should be externalised through environment variables and secure configuration patterns.

Examples:

- BYOM endpoint
- BYOM deployment name
- BYOM API version
- BYOM system prompt
- API key / secret reference where implemented

### Governance Notes

- Do not hardcode endpoints, keys, tokens, tenant values, or secrets.
- Do not publish API keys or endpoint values in screenshots.
- The flow should include fallback behaviour so request creation can continue if the AI call fails.
- The summary is reviewer support, not an automated decision.

---

## GetStatus

### Purpose

GetStatus allows a requester to check the current lifecycle state of their request through Copilot Studio.

It avoids relying on chat memory and returns the authoritative status from Dataverse.

### Trigger

Called by Copilot Studio from the GetStatus topic.

### Main Responsibilities

- Receive request reference or identifier
- Retrieve the matching Access Requests row from Dataverse
- Return current lifecycle state and key timestamps to Copilot Studio

### Typical Inputs

- Request Number or request identifier

### Typical Outputs

- Status
- Status Reason
- SubmittedOn
- DecisionOn
- ResolvedOn
- CaseSummary where appropriate
- Request not found response where appropriate

### Governance Notes

- The flow should return only information the requester is allowed to see.
- In a production design, secure ownership checks would be required.
- Responses should avoid exposing internal-only fields unless appropriate.

---

## Flow B — Approval Watcher

### Purpose

Flow B watches Dataverse for pending requests and sends a Teams Adaptive Card to the approver.

It captures the approver decision and writes it back to Dataverse as structured data.

### Trigger

Dataverse row added or modified.

### Main Responsibilities

- Detect pending requests
- Check whether an approval card has already been sent
- Stamp approval idempotency fields
- Send Teams Adaptive Card to approver
- Capture approval decision and comments
- Update Dataverse with decision outcome

### Key Dataverse Fields Read

- Status
- Status Reason
- ApproverEmail
- ApprovalRequestSent
- ApprovalRequestSentOn
- ApplicationName
- AccessLevel
- Justification
- DurationDays
- CaseSummary
- CorrelationId

### Key Dataverse Fields Updated

- ApprovalRequestSent
- ApprovalRequestSentOn
- ApproverDecision
- ApproverComments
- DecisionOn
- Status
- Status Reason
- ApprovalError where required

### Idempotency Pattern

Approval idempotency is controlled through:

- ApprovalRequestSent
- ApprovalRequestSentOn

Recommended pattern:

1. Check that the request is pending.
2. Check that ApprovalRequestSent is not already true.
3. Stamp ApprovalRequestSent and ApprovalRequestSentOn before posting the card.
4. Send the Teams Adaptive Card.
5. If the watcher retriggers, exit without sending a duplicate card.

### Governance Notes

- The approval decision is stored in Dataverse, not just Teams.
- Approver comments are captured as structured data.
- Duplicate Adaptive Cards are prevented through idempotency.
- Approval errors should be written to ApprovalError.
- Sensitive values should not be exposed in public screenshots.

---

## Flow C — Resolution Watcher

### Purpose

Flow C processes approved and rejected outcomes after the approver decision is written back to Dataverse.

For approved requests, fulfilment is simulated for portfolio purposes.

### Trigger

Dataverse row modified.

### Main Responsibilities

- Detect approved or rejected outcomes
- Process the final outcome
- Perform simulated fulfilment for approved requests
- Close rejected requests without fulfilment
- Stamp resolution fields
- Notify the requester where implemented
- Write errors back to Dataverse

### Key Dataverse Fields Read

- Status
- Status Reason
- ApproverDecision
- ApproverComments
- DecisionOn
- RequestorEmail
- ApplicationName
- AccessLevel
- CorrelationId
- ResolvedOn
- FulfilmentStarted

### Key Dataverse Fields Updated

- Status
- Status Reason
- ResolvedOn
- FulfilmentStarted
- FulfilmentNotes
- FulfilmentError where required
- TimeToResolutionHours where implemented
- PostDecisionResolutionHours where implemented

### Idempotency Pattern

Resolution idempotency can be controlled through:

- ResolvedOn
- FulfilmentStarted
- Status
- Status Reason

Recommended pattern:

1. Detect an approved or rejected decision.
2. Check whether the request has already been resolved.
3. If not resolved, process the outcome.
4. Stamp ResolvedOn and relevant fulfilment/resolution fields.
5. If the watcher retriggers, exit without duplicating fulfilment or notification work.

### Governance Notes

- Fulfilment is mocked or simulated unless future evidence shows real provisioning.
- Real provisioning through Microsoft Graph, Entra groups, application roles, or ITSM integration is future scope.
- Errors should be written to FulfilmentError.
- The lifecycle should close cleanly for both approved and rejected outcomes.

---

## Flow D — SLA Reminder / Escalation Watcher

### Purpose

Flow D is a scheduled watcher that monitors overdue pending approvals.

It demonstrates autonomous operations: the system continues to monitor workflow health even after the requester leaves the chat.

### Trigger

Scheduled recurrence.

### Main Responsibilities

- Find pending requests where approval has been sent but no decision has been recorded
- Determine whether the request is overdue
- Send reminder notifications
- Increment ReminderCount
- Stamp LastReminderOn
- Escalate after configured thresholds
- Set Escalated where appropriate
- Stamp EscalationCount and EscalatedOn

### Key Dataverse Fields Read

- Status
- Status Reason
- ApprovalRequestSent
- ApprovalRequestSentOn
- DecisionOn
- ApproverEmail
- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn

### Key Dataverse Fields Updated

- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn

### SLA / Escalation Pattern

Reminder and escalation behaviour is controlled through:

- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn

Recommended pattern:

1. Run on a schedule.
2. Find pending requests with approval sent and no decision.
3. Check whether the request is overdue.
4. Send a reminder if below threshold.
5. Increment ReminderCount and stamp LastReminderOn.
6. Escalate once configured thresholds are reached.
7. Stamp Escalated, EscalationCount, and EscalatedOn.

There is no separate risk flag field in the current schema. Escalation is tracked through Escalated, EscalationCount, and EscalatedOn.

### Governance Notes

- Scheduled watcher logic demonstrates autonomous operations.
- Reminder and escalation actions are written back to Dataverse.
- One failed record should not stop the entire scheduled sweep where possible.
- Escalation should be controlled to avoid repeated uncontrolled notifications.

---

## Cross-Flow Governance Patterns

### Dataverse as System of Record

Each flow writes lifecycle state back to Dataverse.

This means the request record contains the operational truth, rather than relying only on Teams messages, chat history, or Power Automate run history.

### CorrelationId

CorrelationId is generated in Flow A and can be used to trace the request across:

- Copilot Studio
- Power Automate
- Dataverse
- Teams Adaptive Cards
- Model-driven app investigation

### Error Capture

Errors should be captured in Dataverse fields where possible.

Relevant fields include:

- ApprovalError
- FulfilmentError

This makes operational issues visible in the model-driven app.

### Idempotency

idempotency controls are used to prevent duplicate processing.

Examples:

- ApprovalRequestSent prevents duplicate approval cards.
- ResolvedOn and FulfilmentStarted help prevent duplicate resolution work.
- ReminderCount, LastReminderOn, Escalated, EscalationCount, and EscalatedOn control reminder/escalation behaviour.

### Human-in-the-Loop Approval

The approver remains the decision-maker.

The AI-generated CaseSummary supports review but does not approve or reject access.

### Environment Configuration

Environment-specific values should be externalised through environment variables and connection references.

Do not hardcode:

- API keys
- Client secrets
- Endpoint URLs
- Tenant-specific values
- Channel IDs
- Recipient addresses
- Private connection details

---

## Common Troubleshooting Notes

| Symptom | Likely Area To Check |
|---|---|
| Request created but no approval card appears | Flow B trigger conditions, ApprovalRequestSent, ApproverEmail, Teams connection reference |
| Duplicate approval cards appear | ApprovalRequestSent stamping order and Dataverse trigger filtering |
| CaseSummary is blank | Flow H fallback handling, BYOM environment variables, Azure OpenAI connection, Key Vault configuration where used |
| Status check returns stale information | GetStatus lookup criteria and Dataverse row update timing |
| Approved request processes more than once | Flow C idempotency checks using ResolvedOn and FulfilmentStarted |
| SLA reminders repeat too often | Flow D threshold logic, ReminderCount, LastReminderOn, and recurrence frequency |
| Escalation repeats unexpectedly | Escalated, EscalationCount, and EscalatedOn update logic |
| Error is visible only in run history | ApprovalError and FulfilmentError write-back paths |

These troubleshooting notes are for portfolio review and future maintainers. They should not include private flow run inputs, connection details, endpoint values, or tenant-specific configuration.

---

## Screenshot Checklist

Add redacted screenshots for each flow when ready.

### Essential Screenshots

- Flow A — Create Request overview
- Flow A showing CorrelationId generation
- Flow A calling Flow H — SummariseCase
- Flow H — SummariseCase overview
- Flow H showing BYOM configuration pattern
- Flow H showing fallback handling
- GetStatus helper flow
- Flow B — Approval Watcher overview
- Flow B showing ApprovalRequestSent idempotency check
- Teams Adaptive Card generated by Flow B
- Flow B writing ApproverDecision and ApproverComments to Dataverse
- Flow C — Resolution Watcher overview
- Flow C showing ResolvedOn / FulfilmentStarted logic
- Flow D — SLA Reminder / Escalation Watcher overview
- Flow D showing ReminderCount / LastReminderOn update
- Flow D showing Escalated / EscalationCount / EscalatedOn update

### Nice-to-Have Screenshots

- Flow run history for happy path
- Flow run history for rejection path
- BYOM fallback test
- SLA watcher test run
- Error written to ApprovalError
- Error written to FulfilmentError
- Connection references
- Environment variables
- Managed solution evidence

### Do Not Publish Screenshots Showing

- Tenant URLs
- Environment URLs
- Endpoint URLs
- API keys
- Client secrets
- Bearer tokens
- Connection details
- Personal maker connections
- Real email addresses
- Private approver names
- Private Teams channel names
- Live Dataverse data
- Private run history
- Azure OpenAI endpoint values
- Key Vault secret values

Use demo data and redacted screenshots only.

---

## Flow Documentation Files

Recommended individual flow documentation files:

```text
flows/
├── README.md
├── flow-a-create-request.md
├── flow-h-summarise-case.md
├── flow-get-status.md
├── flow-b-approval-watcher.md
├── flow-c-resolution-watcher.md
└── flow-d-sla-watcher.md
```

Each individual flow file should include:

- Purpose
- Trigger
- Inputs
- Outputs
- Dataverse fields read
- Dataverse fields updated
- Key logic
- Error handling
- Idempotency controls
- Screenshot placeholders
- Known gaps or future improvements

---

## Notes for Recruiters and Reviewers

This flow architecture is designed to show more than simple automation.

The strongest evidence is in the design choices:

- Flow responsibilities are separated.
- Approval logic is outside the chat layer.
- Dataverse holds the lifecycle state.
- Teams is used for human approval, not as the system of record.
- Idempotency prevents duplicate approval and fulfilment actions.
- Errors are written back to Dataverse.
- SLA monitoring demonstrates autonomous operational follow-up.
- BYOM summarisation is reusable and fail-safe.
- Fulfilment is clearly described as simulated.

This is a portfolio/demo project, but the architecture reflects patterns used in enterprise Power Platform solutions.
