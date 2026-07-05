# Project Summary — Access Request & Approval Agent

## Purpose

The **Access Request & Approval Agent** is a Power Platform / Copilot Studio portfolio case study that demonstrates a governed access request and approval lifecycle.

It shows how Copilot Studio can collect structured request details, Power Automate can orchestrate deterministic approval and lifecycle processing, Dataverse can act as the system of record, Teams Adaptive Cards can capture human-in-the-loop approval, and Azure OpenAI BYOM summarisation can support reviewers without making the approval decision.

This is a portfolio/demo project. It is not a production access management system and does not replace Microsoft Entra ID Governance, a production identity governance platform, or an enterprise ITSM/access management tool.

Fulfilment/provisioning is mocked or simulated unless future evidence shows a real provisioning integration.

---

## Business Problem

Access requests are often submitted through informal email, chat, spreadsheet, or manual ticketing processes.

That can lead to:

- Incomplete or inconsistent request information
- Approvals lost in email or chat threads
- Duplicate approval notifications when workflows retrigger
- Weak audit trails
- Unclear ownership
- Limited requester visibility after submission
- Overdue approvals sitting unnoticed
- Support teams relying on flow run history rather than structured business records

This project demonstrates a more governed pattern where the user experience remains simple and conversational, but the lifecycle behind it is structured, auditable, and supportable.

---

## Portfolio Positioning

Position this project as an **enterprise-aligned Power Platform / Copilot Studio portfolio case study**.

It demonstrates:

- Copilot Studio agent design
- Dataverse-first lifecycle architecture
- Power Automate watcher patterns
- Teams Adaptive Card approval
- Entra ID-backed secure identity capture
- BYOM Azure OpenAI summarisation
- RBAC and auditability
- Idempotency and error capture
- SLA reminder and escalation logic
- Operational reporting fields
- ALM and release discipline

Do not position this as a live production access management platform. A real production deployment would require further security review, tenant-specific DLP validation, operational monitoring, environment hardening, and real provisioning integration.

---

## Target Users / Personas

### Requester

An employee or demo user who needs temporary access to an application, system, or admin function.

The requester uses Copilot Studio to submit the access request and can use GetStatus to check progress later.

### Approver

A manager, service owner, or authorised reviewer who receives a Teams Adaptive Card, reviews the request details and CaseSummary, then approves or rejects with comments.

### Fulfilment / Operations User

A back-office or IT operations user who reviews request outcomes, simulated fulfilment state, error fields, reminder activity, escalation state, and resolution metrics in the model-driven app.

### Platform / Governance Admin

A Power Platform administrator or governance reviewer who checks RBAC, audit history, environment variables, connection references, ALM evidence, and release readiness.

### Portfolio Reviewer

A recruiter, Microsoft Partner reviewer, hiring manager, or technical interviewer assessing the project as evidence of Power Platform, Copilot Studio, Dataverse, Power Automate, governance, and secure agent design skills.

---

## Main User Journeys

### 1. Submit Access Request

The requester uses the Access Request Agent to provide:

- Application name
- Access level
- Business justification
- Duration
- Approver email

If the requested access level is Admin, the agent shows a policy acknowledgement gate before submission.

Flow A — Create Request validates the request, generates a CorrelationId, calls Flow H — SummariseCase, creates the Dataverse Access Requests row, and returns the request status to Copilot Studio.

### 2. Approve or Reject Request

Flow B — Approval Watcher monitors Dataverse for pending requests.

When a new pending request is detected and no approval card has already been sent, the flow sends a Teams Adaptive Card to the approver.

The approver reviews:

- Application requested
- Access level
- Duration
- Justification
- CaseSummary

The approver then submits:

- ApproverDecision
- ApproverComments

The decision is written back to Dataverse as structured data.

### 3. Resolve Request

Flow C — Resolution Watcher processes approved and rejected outcomes.

For approved requests, the flow performs mocked or simulated fulfilment and stamps the relevant completion fields.

For rejected requests, the flow records the rejected outcome and closes the lifecycle without fulfilment.

In both cases, Dataverse remains the authoritative lifecycle record.

### 4. Check Request Status

The requester can ask the agent to check a request status.

The GetStatus helper flow retrieves the latest lifecycle state from Dataverse and returns the current Status, Status Reason, and key timestamps to Copilot Studio.

This avoids relying on chat memory or manual support follow-up.

### 5. Monitor SLA and Escalation

Flow D — SLA Reminder / Escalation Watcher runs on a schedule.

It checks pending approvals, sends reminders, increments ReminderCount, stamps LastReminderOn, and escalates after configured thresholds.

Escalation activity is written back to Dataverse using Escalated, EscalationCount, and EscalatedOn so operational follow-up is visible and auditable.

---

## Agent Behaviour

The agent acts as a governed intake and status interface.

It should:

- Collect required request details conversationally
- Validate key fields before calling Power Automate
- Apply an Admin-access policy acknowledgement gate
- Use signed-in identity in the secure variant
- Submit structured data to Power Automate
- Return a clear confirmation to the requester
- Support request status checks through GetStatus
- Avoid making approval decisions itself
- Avoid provisioning access directly

The agent is the user-facing front door. Power Automate and Dataverse control the business lifecycle.

---

## Conversation Flow

### Request Access Topic

1. User asks to request access.
2. Agent asks for application name.
3. Agent asks for access level.
4. If access level is Admin, agent displays a policy acknowledgement message.
5. Agent asks for business justification.
6. Agent validates the justification.
7. Agent asks for duration.
8. Agent validates the duration.
9. Agent captures requester identity.
   - Public mode: requester email can be collected conversationally.
   - Secure mode: requester identity is captured from Entra ID signed-in context.
10. Agent asks for approver email.
11. Agent calls Flow A — Create Request.
12. Agent returns request confirmation, status, and relevant identifiers.

### GetStatus Topic

1. User asks to check request status.
2. Agent asks for the request reference or identifier.
3. Agent calls GetStatus.
4. GetStatus retrieves the current Dataverse record.
5. Agent returns the current lifecycle state and key timestamps.

---

## Core Components

| Component | Role |
|---|---|
| Access Request Agent (Public) | Public/demo conversational intake with manually supplied requester identity |
| Access Request Agent (Internal Auth / Secure) | Secure mode using Entra ID identity capture |
| Access Request (Secure) | Secure variant name used for authenticated demonstration |
| Flow A — Create Request | Creates the Dataverse Access Requests row and returns request details |
| Flow H — SummariseCase | Generates CaseSummary through Azure OpenAI BYOM with fallback handling |
| GetStatus | Returns current request status from Dataverse |
| Flow B — Approval Watcher | Sends Teams Adaptive Card approval and captures the decision |
| Flow C — Resolution Watcher | Processes approved/rejected outcomes and simulated fulfilment |
| Flow D — SLA Reminder / Escalation Watcher | Monitors overdue approvals and records reminders/escalations |
| Dataverse Access Requests table | System of record for lifecycle state, decisions, timestamps, errors, traceability, and reporting metrics |
| Model-driven app | Operational view of requests, errors, audit fields, SLA state, and resolution metrics |
| Teams Adaptive Cards | Human approval interface |
| Azure OpenAI BYOM | Reviewer-support CaseSummary generation |
| Key Vault / environment variables | Secure and configurable BYOM pattern |
| Power Platform solution | ALM container for components, connection references, and environment-specific configuration |

---

## Power Automate Flows

The project uses six focused flows rather than one large orchestration flow.

### Flow A — Create Request

Copilot-triggered flow.

Responsibilities:

- Validate request inputs
- Generate CorrelationId
- Call Flow H — SummariseCase
- Create Dataverse Access Requests row
- Set initial lifecycle state
- Return structured confirmation to Copilot Studio

### Flow H — SummariseCase

Reusable BYOM child flow.

Responsibilities:

- Build reviewer-support summary from request details
- Use environment-variable-driven model configuration
- Use Key Vault-backed secret handling pattern where implemented
- Return CaseSummary to Flow A
- Return fallback summary if AI call fails

The AI summary supports the reviewer. It does not approve, reject, or provision access.

### GetStatus

Helper flow.

Responsibilities:

- Receive request reference from Copilot Studio
- Retrieve request record from Dataverse
- Return current Status, Status Reason, and key lifecycle timestamps
- Support requester self-service status checks

### Flow B — Approval Watcher

Dataverse-triggered watcher.

Responsibilities:

- Detect pending requests
- Check ApprovalRequestSent before posting
- Send one Teams Adaptive Card to the approver
- Capture ApproverDecision and ApproverComments
- Write DecisionOn and decision fields back to Dataverse
- Prevent duplicate approval cards through idempotency

### Flow C — Resolution Watcher

Dataverse-triggered watcher.

Responsibilities:

- Process approved and rejected outcomes
- Perform simulated fulfilment for approved requests
- Close rejected requests without fulfilment
- Stamp ResolvedOn and other resolution fields where appropriate
- Notify the requester where implemented
- Write errors back to Dataverse

### Flow D — SLA Reminder / Escalation Watcher

Scheduled watcher.

Responsibilities:

- Find overdue pending approvals
- Send reminder notifications
- Increment ReminderCount
- Stamp LastReminderOn
- Escalate after configured thresholds
- Set Escalated where appropriate
- Stamp EscalationCount and EscalatedOn
- Continue processing other records if one item fails

---

## Dataverse System of Record

The **Access Requests** table holds the authoritative lifecycle state for each request.

The table is designed so that key lifecycle events, decisions, timestamps, errors, reminders, escalation status, and reporting metrics are visible from the Dataverse record rather than only inside Power Automate run history.

Important fields include:

### Request Identity and Reference

- Request Number
- RequestName
- Request Title (Primary)
- CorrelationId

### Requester and Approver Identity

- RequestorEmail
- RequestorAadObjectId
- ApproverEmail

### Request Details

- ApplicationName
- AccessLevel
- Justification
- DurationDays

### Lifecycle State

- Status
- Status Reason
- SubmittedOn
- DecisionOn
- ResolvedOn

### Approval Tracking

- ApprovalRequestSent
- ApprovalRequestSentOn
- ApproverDecision
- ApproverComments
- ApprovalError

### Fulfilment / Resolution Tracking

- FulfilmentStarted
- FulfilmentNotes
- FulfilmentError

### AI Summary

- CaseSummary

### Traceability

- CorrelationId

### SLA / Reminder / Escalation Tracking

- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn

### Reporting and Operational Metrics

- TimeToApproveHours
- TimeToResolutionHours
- PostDecisionResolutionHours

### Standard Dataverse Ownership and Audit Fields

- Owner
- Created By
- Created On
- Modified By
- Modified On
- Owning Business Unit
- Owning Team
- Owning User

There is no separate risk flag field in the current schema. Escalation is tracked through Escalated, EscalationCount, and EscalatedOn.

---

## Inputs and Outputs

### Main Inputs

From the requester:

- Application name
- Access level
- Business justification
- Duration
- Approver email

From secure identity context:

- RequestorEmail / UPN
- RequestorAadObjectId

From the approver:

- ApproverDecision
- ApproverComments

From configuration:

- BYOM endpoint
- BYOM deployment name
- BYOM API version
- BYOM system prompt
- SLA reminder threshold
- Maximum reminder count
- Escalation recipient
- Approval routing destination

### Main Outputs

To Dataverse:

- Access Request row
- Request Number
- Status and Status Reason updates
- CaseSummary
- CorrelationId
- Approval decision fields
- Resolution / fulfilment fields
- SLA reminder fields
- Escalation fields
- Error fields
- Reporting metrics

To Copilot Studio:

- Request confirmation
- Request status
- CaseSummary or summary text where appropriate
- Current lifecycle state from GetStatus

To Teams:

- Adaptive Card approval request
- Reminder or escalation messages where implemented

To requester:

- Confirmation and status responses
- Resolution / fulfilment notification where implemented

---

## Governance Themes

### RBAC and Least Privilege

Security roles separate requester, approver, fulfilment/operations, and admin responsibilities.

The intended pattern is least privilege: users only receive access required for their role in the lifecycle.

### Draft-Only Editing

Draft-only editing prevents requesters from changing core request details after the process has started.

This protects the integrity of the approval decision.

### Field Security Profile

Field-level security can protect internal/system fields such as CorrelationId, RequestorAadObjectId, CaseSummary, error fields, and fulfilment notes.

Only describe this as fully implemented where evidence exists.

### Dataverse Audit History

Dataverse audit history supports traceability for key lifecycle changes, including approval decisions, status changes, fulfilment fields, error fields, SLA counters, and escalation state.

### Idempotency

idempotency flags prevent duplicate processing.

Examples:

- ApprovalRequestSent prevents duplicate Teams approval cards.
- FulfilmentStarted or ResolvedOn can prevent duplicate fulfilment/resolution processing.
- ReminderCount and LastReminderOn help control reminder behaviour.
- Escalated helps identify records that have already entered escalation state.

### Error Capture

Flows use TRY/CATCH-style handling patterns.

Failures are written back to Dataverse error fields so issues are visible from the business record, not only inside Power Automate run history.

### CorrelationId

CorrelationId is generated at request creation and stored on the record.

It supports cross-system tracing between Copilot Studio, Power Automate run history, Dataverse, Teams messages, and support investigation.

### Configuration and ALM

The project demonstrates ALM discipline through:

- Environment variables
- Connection references
- Managed and unmanaged solution separation
- Release checklist
- Definition of Done
- No hardcoded endpoint, key, channel, or routing values where possible

---

## Human Review Points

Human review remains central to the design.

The system does not approve access automatically.

Human review occurs when:

- The requester acknowledges the policy gate for Admin-level access.
- The approver reviews the Teams Adaptive Card.
- The approver submits ApproverDecision and ApproverComments.
- Operations/admin users review request state, errors, audit history, metrics, or SLA activity in the model-driven app.

---

## Error Handling and Edge Cases

Expected handling patterns include:

### Invalid or Incomplete Request

The agent or Flow A should prevent incomplete or invalid requests from entering the lifecycle.

### Admin Access Request

Admin access triggers a policy acknowledgement gate before the request proceeds.

### AI Summary Failure

Flow H should return a safe fallback summary so request creation can continue.

The AI layer enriches the process but should not block the core transaction.

### Duplicate Approval Watcher Trigger

Flow B checks ApprovalRequestSent before posting a Teams Adaptive Card.

This prevents duplicate approval notifications if Dataverse retriggers the flow.

### Approval Card Failure

Approval card failure should be written to ApprovalError or another dedicated error field.

### Fulfilment Failure

Fulfilment or resolution failure should be written to FulfilmentError.

### Overdue Approval

Flow D identifies overdue pending approvals and records reminder/escalation activity.

### Request Already In Escalation

Flow D can use Escalated, EscalationCount, and EscalatedOn to avoid repeatedly escalating the same request without control.

### Missing Secure Identity Claim

The secure variant can include fallback handling if the expected signed-in email claim is blank.

---

## Current Build Status

This repo documents a portfolio-ready case study and public GitHub foundation.

The documented project includes:

- Copilot Studio conversational intake
- Public/demo and secure/enterprise modes
- Entra ID identity capture in the secure variant
- Dataverse Access Requests table
- Six-flow Power Automate architecture
- Teams Adaptive Card approval
- BYOM CaseSummary pattern
- GetStatus status-check helper
- Flow C — Resolution Watcher with simulated fulfilment
- SLA reminder and escalation watcher
- Model-driven app visibility
- RBAC, audit, idempotency, error capture, reporting metrics, and ALM documentation

The repo should distinguish between:

- Built or demonstrated features
- Demo-only or mocked features
- Planned enhancements
- Future production improvements

---

## Known Gaps

Known gaps or areas to be careful about:

- Real access provisioning is not implemented.
- Fulfilment is simulated for portfolio purposes.
- Microsoft Graph integration is a future improvement.
- Production DLP policy evidence is not included unless added later.
- Full production monitoring and alerting are not included.
- Field-level security should only be described as implemented where supporting evidence exists.
- Public screenshots must be redacted before publishing.
- No secrets, API keys, client secrets, endpoint values, tenant URLs, or live user data should be published.

---

## Future Improvements

Potential future improvements include:

- Real provisioning through Microsoft Graph
- Entra group membership assignment on approval
- Application role assignment
- Approver validation against Entra ID
- Auto-resolving manager or service owner from Entra ID
- Approval delegation for out-of-office scenarios
- Power BI reporting for request volume, SLA compliance, approval turnaround, and resolution time
- Managed Environment and DLP policy evidence
- CoE Starter Kit / governance dashboard integration
- Production-grade monitoring and alerting
- More detailed automated testing evidence
- Support dashboard for failed or stuck requests
- Full managed solution deployment evidence across Dev/Test/Prod-style environments

---

## Repository Notes for Codex

When editing this repo:

- Keep the project positioned as a portfolio/demo case study.
- Do not overclaim production readiness.
- Clearly distinguish built, demo-only, planned, and future features.
- Preserve the names of flows, tables, fields, and agent variants.
- Use the actual schema names from the Access Requests table.
- Use ResolvedOn rather than legacy fulfilment-completion field naming.
- Preserve implementation field names exactly: FulfilmentError, FulfilmentNotes, and FulfilmentStarted.
- Use Escalated, EscalationCount, and EscalatedOn for escalation tracking.
- Do not introduce a separate risk flag field; risk scoring is not part of the current schema.
- Do not publish private local notes.
- Do not include secrets, tenant-specific details, personal email addresses, live endpoint URLs, or private screenshots.
- Use placeholder text where screenshots or implementation evidence still needs to be added.
- Keep fulfilment described as mocked/simulated unless real provisioning evidence is added later.

---

## Summary

The Access Request & Approval Agent demonstrates a governed access request lifecycle using Copilot Studio, Power Automate, Dataverse, Teams, Entra ID, Azure OpenAI BYOM summarisation, and model-driven app visibility.

Its strongest portfolio value is not the chat interface alone. The value is the governed lifecycle behind it: structured intake, deterministic orchestration, human approval, Dataverse auditability, idempotency, error capture, SLA monitoring, escalation tracking, secure identity patterns, operational metrics, and ALM discipline.
