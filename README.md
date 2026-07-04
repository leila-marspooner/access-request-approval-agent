# Access Request & Approval Agent

**Power Platform / Copilot Studio portfolio case study demonstrating a governed access request lifecycle with conversational intake, Teams approval, Dataverse tracking, Entra ID identity capture, BYOM AI summarisation, SLA monitoring, and audit-friendly governance controls.**

This project shows how a Copilot Studio agent can act as the front door to a governed business-process pattern, while Power Automate and Dataverse provide the control, traceability, and operational reliability behind the scenes.

---

## Recruiter Snapshot

| Area | What This Project Demonstrates |
|---|---|
| Copilot Studio | Conversational intake, structured validation, policy acknowledgement, status checks, public and secure agent modes |
| Power Automate | Six focused flows for request creation, AI summarisation, approval, resolution, status lookup, and SLA monitoring |
| Dataverse | System of record for lifecycle state, decisions, timestamps, errors, escalation, and reporting metrics |
| Teams | Human-in-the-loop approval through Adaptive Cards |
| Entra ID | Secure variant captures signed-in requester identity rather than relying only on typed email |
| Azure OpenAI BYOM | Reusable CaseSummary child flow with environment-variable configuration and fallback handling |
| Governance | RBAC, draft-only editing, idempotency, audit history, correlation IDs, error capture, and ALM discipline |

---

## One-Minute Overview

A requester asks for access in Copilot Studio. The agent collects structured details such as application, access level, justification, duration, and approver.

Flow A — Create Request creates a Dataverse Access Requests record and generates a CorrelationId. Flow H — SummariseCase creates a reviewer-support CaseSummary using a BYOM Azure OpenAI pattern. Flow B — Approval Watcher sends a Teams Adaptive Card for human approval. Flow C — Resolution Watcher records the approved or rejected outcome and performs simulated fulfilment where appropriate. Flow D — SLA Reminder / Escalation Watcher monitors overdue approvals. GetStatus lets the requester check progress directly from Dataverse.

The public agent variant is designed for easy portfolio review. The secure variant demonstrates Entra ID-backed identity capture for a more enterprise-aligned pattern.

---

## Portfolio Positioning

This is a **portfolio/demo project** built to demonstrate enterprise-aligned Power Platform design patterns.

It is not a production access management system and does not replace:

- Microsoft Entra ID Governance
- A production identity governance platform
- An enterprise ITSM/access management tool
- A real provisioning workflow

Fulfilment/provisioning is mocked or simulated unless future evidence shows a real provisioning integration.

The purpose of the repo is to show how I approach:

- Business process design
- Copilot Studio agent design
- Dataverse-first architecture
- Power Automate orchestration
- Secure identity patterns
- Human approval workflows
- Governance and auditability
- ALM and public documentation

---

## Business Problem

Internal access requests are often handled through email, chat, spreadsheets, or informal ticketing processes. This creates predictable governance and operational issues:

- Requests are inconsistent or missing key details.
- Approval decisions are difficult to trace.
- Duplicate approval notifications can be sent when automation retriggers.
- Requesters have limited visibility after submission.
- Overdue approvals can sit unnoticed.
- Support teams may need flow admin access just to investigate failures.
- There may be no reliable way to prove who requested access, who approved it, and when.

This project demonstrates a more governed pattern: a simple conversational experience for the requester, backed by structured Dataverse records, deterministic Power Automate flows, Teams approval, and audit-friendly lifecycle tracking.

---

## Key Capabilities

### Agent Experience

- Copilot Studio conversational intake for access requests
- Structured validation before automation begins
- Admin-access policy acknowledgement gate
- Public/demo mode and secure/enterprise mode
- GetStatus status-check topic
- Entra ID identity capture in the secure variant

### Workflow Automation

- Six-flow Power Automate architecture
- Create Request flow
- Reusable BYOM SummariseCase child flow
- GetStatus helper flow
- Approval Watcher using Teams Adaptive Cards
- Resolution Watcher with simulated fulfilment
- SLA Reminder / Escalation Watcher

### Dataverse and Operations

- Access Requests table as the system of record
- Request Number for user-facing tracking
- Status and Status Reason lifecycle fields
- CaseSummary and CorrelationId for review and traceability
- ApprovalRequestSent and ApprovalRequestSentOn for approval idempotency
- ResolvedOn and fulfilment fields for lifecycle closure
- ReminderCount, LastReminderOn, EscalatedFlag, EscalationCount, and EscalatedOn for SLA tracking
- TimeToApproveHours, TimeToResolutionHours, and PostDecisionResolutionHours for reporting

### Security and Governance

- Role-Based Access Control
- Draft-only editing guardrail
- Field Security Profile evidence where implemented
- Dataverse audit history
- Error capture through ApprovalError and FulfillmentError
- CorrelationId for cross-system tracing
- Environment-variable-driven configuration
- Connection references
- Managed and unmanaged solution separation
- Release checklist discipline

---

## Architecture Overview

The build is intentionally modular.

```text
Requester
  -> Copilot Studio Agent
  -> Flow A — Create Request
  -> Flow H — SummariseCase
  -> Dataverse Access Requests
  -> Flow B — Approval Watcher
  -> Teams Adaptive Card
  -> Dataverse decision update
  -> Flow C — Resolution Watcher
  -> Flow D — SLA Reminder / Escalation Watcher
  -> Model-driven app visibility
```

### Architecture Diagram

> Diagram to be added: `architecture/access-request-architecture.png`

When the architecture diagram is added, use:

```markdown
![Access Request & Approval Agent architecture](architecture/access-request-architecture.png)
```

The diagram should show the request lifecycle from Copilot Studio through Power Automate, Dataverse, Teams Adaptive Cards, the model-driven app, Entra ID, and Azure OpenAI / Key Vault.

---

## Core Components

| Component | Purpose |
|---|---|
| Copilot Studio | Conversational intake, validation, policy acknowledgement, GetStatus checks, and public/secure agent modes |
| Power Automate | Deterministic orchestration for request creation, approval, resolution, reminders, retries, and error capture |
| Dataverse | System of record for request lifecycle, decisions, timestamps, errors, SLA fields, and metrics |
| Teams Adaptive Cards | Human-in-the-loop approval with structured decision capture |
| Azure OpenAI BYOM | CaseSummary generation as reviewer support, not the decision-maker |
| Key Vault | Secure handling pattern for sensitive BYOM configuration |
| Model-driven app | Operational visibility over status, audit history, errors, reminders, escalation, and reporting metrics |
| ALM | `lai_AccessRequestAgent` solution packaging, environment variables, connection references, and managed/unmanaged solution discipline |

---

## Power Automate Flows

The project uses six focused Power Automate flows rather than one large orchestration flow.

| Flow | Purpose |
|---|---|
| Flow A — Create Request | Copilot-triggered flow that validates inputs, generates a CorrelationId, calls SummariseCase, creates the Dataverse record, and returns structured outputs to the agent |
| Flow H — SummariseCase | Reusable BYOM child flow that generates a short CaseSummary using environment-variable-driven model configuration |
| GetStatus | Helper flow that returns the current lifecycle state from Dataverse to Copilot Studio |
| Flow B — Approval Watcher | Dataverse-triggered watcher that posts one Teams Adaptive Card and writes the approver decision back to Dataverse |
| Flow C — Resolution Watcher | Processes approved and rejected outcomes, stamps resolution and fulfilment fields, notifies the requester where implemented, and logs failures |
| Flow D — SLA Reminder / Escalation Watcher | Scheduled watcher that detects overdue approvals, sends reminders, increments counters, sets EscalatedFlag where appropriate, and escalates after a threshold |

---

## Why Power Automate Instead of Agent Flows for Approval Logic?

Approval, resolution, SLA, and fulfilment logic are implemented in Power Automate rather than being handled directly in the chat layer.

This was a deliberate governance and reliability decision. Approval workflows require:

- Deterministic branching
- Retry control
- Run history
- Idempotency
- Error capture
- Auditable Dataverse write-back
- Supportability outside the conversation layer

Copilot Studio is used for user interaction. Power Automate and Dataverse control the business process.

---

## Public Demo Mode vs Secure Enterprise Mode

| Mode | Purpose | Identity Pattern |
|---|---|---|
| Access Request Agent (Public) | Low-friction portfolio review and demo testing | Requester email can be collected conversationally |
| Access Request Agent (Internal Auth / Secure) | Enterprise-aligned pattern using Entra ID | RequestorEmail / UPN and RequestorAadObjectId are captured from signed-in identity |

Building both modes is intentional.

The public version makes the project easier for reviewers to understand and test. The secure version demonstrates how the same pattern can be adapted for identity-aware enterprise scenarios without trusting user-typed identity data.

---

## Dataverse Schema Highlights

The **Access Requests** table is the authoritative system of record.

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

- FulfillmentStarted
- FulfillmentNotes
- FulfillmentError

### AI Summary

- CaseSummary

### SLA / Reminder / Escalation Tracking

- ReminderCount
- LastReminderOn
- EscalatedFlag
- EscalationCount
- EscalatedOn

### Reporting and Operational Metrics

- TimeToApproveHours
- TimeToResolutionHours
- PostDecisionResolutionHours

There is no separate risk flag field in the current schema. Escalation is tracked through EscalatedFlag, EscalationCount, and EscalatedOn.

---

## Demo Walkthrough

A typical walkthrough shows:

1. Requester opens the Copilot Studio agent.
2. Requester asks for access to an application.
3. Agent collects application, access level, justification, duration, and approver.
4. Admin-level access triggers a policy acknowledgement gate.
5. Flow A creates the Dataverse request record.
6. Flow H generates a CaseSummary.
7. Dataverse stores the request with Pending status and CorrelationId.
8. Flow B sends a Teams Adaptive Card to the approver.
9. Approver approves or rejects with comments.
10. Dataverse stores ApproverDecision, ApproverComments, and DecisionOn.
11. Flow C processes the outcome and simulates fulfilment where appropriate.
12. Flow D demonstrates overdue approval reminder and escalation logic.
13. The model-driven app shows the full lifecycle state, error fields, SLA fields, and audit evidence.

---

## What Recruiters and Reviewers Should Look For

This repo is intended to show more than a working chatbot.

The strongest evidence is in the design patterns:

- The agent does not own the approval decision.
- Dataverse holds the authoritative state.
- Power Automate handles deterministic workflow steps.
- The approval watcher uses idempotency to prevent duplicate cards.
- AI is isolated in a reusable child flow with fallback handling.
- Secure mode captures signed-in Entra ID identity.
- SLA monitoring demonstrates autonomous operational behaviour.
- Errors are written back to Dataverse for supportability.
- ALM documentation shows awareness of release discipline.

---

## Built, Demo-Only, and Future Scope

### Built / Demonstrated

- Copilot Studio request intake
- Public/demo and secure/enterprise agent modes
- Entra ID-backed identity capture in the secure variant
- Dataverse Access Requests lifecycle tracking
- Six focused Power Automate flows
- Teams Adaptive Card approval
- BYOM CaseSummary generation with fallback handling
- SLA reminder and escalation watcher
- Model-driven app visibility
- RBAC, audit, error capture, idempotency, operational metrics, and ALM documentation

### Demo-Only / Mocked

- Fulfilment/provisioning is simulated.
- Public mode may use user-entered requester email for portfolio accessibility.
- Screenshots and sample records should use demo data.
- The repo documents the architecture and evidence pack; it does not represent a live production access management service.

### Future Scope

- Real provisioning through Microsoft Graph, Entra ID groups, application roles, or an ITSM platform
- Approver validation against Entra ID
- Auto-resolving group membership or application role assignment
- Delegated approvals for out-of-office scenarios
- Power BI reporting for request volume, SLA compliance, approval turnaround, and resolution time
- Managed Environment and DLP policy evidence
- Production-grade monitoring and alerting

---

## Security and Governance Features

- Role-Based Access Control for requester, approver, fulfilment/operations, and admin roles
- Public demo mode for low-friction portfolio testing
- Secure mode using Entra ID manual authentication
- Requester identity captured from signed-in context in the secure variant
- Draft-only editing guardrails once a request leaves Draft
- Field-level security for internal/system fields where implemented
- Dataverse audit history for approval, fulfilment, error, SLA, escalation, and operational metric fields
- Idempotency flags to prevent duplicate Teams cards and duplicate fulfilment/resolution processing
- CorrelationId for cross-system tracing
- Environment-variable-driven configuration
- Connection references for connector abstraction
- Error fields written back to Dataverse
- Approval decisions stored as structured Dataverse data, not just Teams/chat history

---

## ALM and Release Discipline

This repo includes ALM documentation for a production-style release approach.

The intended solution identity is:

`lai_AccessRequestAgent`

ALM principles:

- Unmanaged solution is used for development.
- Managed solution is used for UAT/production-style deployment.
- Environment-specific values are externalised through environment variables.
- Connectors are abstracted through connection references.
- No endpoints, keys, channel IDs, or recipient addresses should be hardcoded in flow actions.
- Versions use MAJOR.MINOR.PATCH.BUILD.
- Previous managed solution packages should be retained as rollback artefacts.
- Release evidence should include environment variables, connection references, RBAC evidence, audit screenshots, flow run history, idempotency proof, and BYOM fallback proof.

---

## Repository Scope

This repository documents the architecture, design decisions, screenshots, demo evidence, and portfolio write-up for the project.

It does not currently include tenant-specific configuration exports, secrets, live Dataverse data, or production deployment artefacts.

If solution files are added in the future, they should be reviewed carefully before publishing and must not include secrets, personal connections, unmanaged private exports, tenant-specific values, or live business data.

---

## Current Status

This repo documents a portfolio case study and public GitHub foundation for the Access Request & Approval Agent.

The documentation distinguishes built or demonstrated patterns from demo-only, mocked/simulated, planned, and future improvement items.

Mock fulfilment is used to demonstrate the approval-to-resolution lifecycle without granting real access. Real provisioning through Microsoft Graph, Entra ID groups, application roles, or an ITSM platform is a future improvement.

---

## Repository Structure

```text
access-request-approval-agent/
├── AGENTS.md
├── README.md
├── .gitignore
├── docs/
├── architecture/
├── agents/
├── flows/
├── dataverse/
├── screenshots/
├── demo/
├── assets/
└── source-notes/  # private/ignored
```

---

## Documentation

- [Project summary](docs/project-summary.md)
- [Architecture](docs/architecture.md)
- [Demo script](docs/demo-script.md)
- [Build notes](docs/build-notes.md)
- [ALM release checklist](docs/alm-release-checklist.md)
- [Security and governance](docs/security-governance.md)
- [Future improvements](docs/future-improvements.md)
- [Agent design](agents/README.md)
- [Flow documentation](flows/README.md)
- [Dataverse documentation](dataverse/README.md)
- [Demo walkthrough](demo/walkthrough.md)
- [Screenshot guidance](screenshots/README.md)

---

## Screenshot Safety

Screenshots should be redacted before publishing.

Do not show:

- Tenant IDs
- Tenant URLs
- Environment URLs
- Private endpoint URLs
- Real email addresses
- Private approver names
- Client IDs
- Secrets
- Bearer tokens
- Connection details
- Live Dataverse data
- Private Power Automate run history
- Azure OpenAI endpoint values
- Key Vault secret values
- Browser tabs or bookmarks containing private information

Use placeholders where evidence still needs to be added.

---

## Suggested Screenshots

### Essential

- Copilot Studio request topic
- Admin-access policy acknowledgement gate
- Secure sign-in prompt
- Dataverse Access Requests row
- Request Number and status evidence
- Teams Adaptive Card approval
- Approval decision written back to Dataverse
- Flow A — Create Request
- Flow H — SummariseCase
- Flow B — Approval Watcher idempotency check
- Flow C — Resolution Watcher
- Flow D — SLA Reminder / Escalation Watcher
- Model-driven app views
- SLA / escalation fields: ReminderCount, LastReminderOn, EscalatedFlag, EscalationCount, EscalatedOn
- Reporting metrics: TimeToApproveHours, TimeToResolutionHours, PostDecisionResolutionHours
- RBAC role evidence
- Audit history evidence
- Environment variables and connection references

### Nice to Have

- Architecture diagram
- Demo website channel
- Secure agent authentication settings
- Redacted Entra app registration evidence
- BYOM fallback run
- SLA reminder run history
- ALM release checklist evidence

---

## Known Gaps / Future Improvements

- Replace mock fulfilment with real provisioning integration.
- Use Microsoft Graph to validate approver email and resolve group membership.
- Add approval delegation for out-of-office scenarios.
- Add Power BI reporting for request volume, SLA compliance, approval turnaround, and resolution time.
- Add production-grade DLP policy evidence.
- Add monitoring and alerting for failed flows.
- Package and test managed releases across Dev/Test/Prod-style environments.
- Expand automated testing evidence.
- Add public architecture diagram once finalised.

---

## Disclaimer

This is a portfolio project built to demonstrate Power Platform, Copilot Studio, Dataverse, Power Automate, Teams, Entra ID, governance, and secure agent design patterns.

It is not a production access management system. Real-world deployment would require additional security review, DLP policy configuration, tenant-specific identity governance, production monitoring, and integration with approved provisioning systems.