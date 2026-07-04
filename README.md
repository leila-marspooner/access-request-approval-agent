# Access Request & Approval Agent

A Power Platform / Copilot Studio portfolio case study showing a governed access request and approval lifecycle.

The Access Request & Approval Agent demonstrates how Copilot Studio can handle conversational intake while Power Automate handles deterministic orchestration, Dataverse acts as the system of record, Teams Adaptive Cards provide Human-in-the-loop approval, Azure OpenAI BYOM summarisation supports reviewers, and watcher flows monitor approval, resolution, and SLA activity.

This is a portfolio/demo project, not a production access management system. It does not replace Microsoft Entra ID Governance, a production identity governance platform, or an enterprise ITSM/access management tool. Fulfilment/provisioning is mocked or simulated unless future evidence shows a real provisioning integration.

## 60-Second Overview

A requester asks for access in Copilot Studio. The agent collects structured details, Flow A — Create Request creates a Dataverse Access Requests record, Flow H — SummariseCase creates a reviewer-support CaseSummary, Flow B — Approval Watcher sends a Teams Adaptive Card for human approval, and Flow C — Resolution Watcher records the approved/rejected outcome with simulated fulfilment where appropriate. Flow D — SLA Reminder / Escalation Watcher monitors overdue approvals, while GetStatus lets the requester check progress from Dataverse.

Access Request Agent (Public) is for easy portfolio review. Access Request Agent (Internal Auth / Secure) demonstrates Entra ID-backed identity capture for a more enterprise-aligned pattern.

## What This Demonstrates

- Copilot Studio conversational intake for access requests
- Public/demo mode and secure/enterprise mode
- Entra ID identity capture in the secure variant
- Dataverse Access Requests table as the system of record
- Six-flow Power Automate architecture
- Teams Adaptive Card approval
- BYOM Azure OpenAI CaseSummary generation
- Key Vault and Environment variables for configuration
- Approval Idempotency with ApprovalRequestSent and ApprovalRequestSentOn
- Resolution / mock fulfilment watcher
- SLA reminder and escalation watcher
- GetStatus status-check flow
- Model-driven app operational visibility
- RBAC, Field Security Profile, Dataverse audit history, Error capture, and CorrelationIds
- ALM discipline through solution packaging, Connection references, Environment variables, Managed solution and Unmanaged solution separation, and a Release checklist

## Solution Overview

The requester starts in Copilot Studio and provides the application, access level, justification, duration, and approver details. In Access Request Agent (Public), requester email can be captured conversationally for a low-friction demo. In Access Request Agent (Internal Auth / Secure), the secure variant uses Entra ID context to capture RequestorEmail / UPN and RequestorAadObjectId.

Flow A — Create Request validates the request, generates a CorrelationId, calls Flow H — SummariseCase, creates the Dataverse Access Requests row, and returns the request status to the agent. Approval, resolution, SLA monitoring, and fulfilment logic are deliberately implemented in Power Automate rather than Agent Flows because those steps need deterministic control, Idempotency, retry handling, run history, Error capture, and Dataverse write-back.

Flow B — Approval Watcher sends a Teams Adaptive Card to the approver and writes ApproverDecision, ApproverComments, and DecisionOn back to Dataverse. Flow C — Resolution Watcher handles approved and rejected outcomes, including simulated fulfilment. Flow D — SLA Reminder / Escalation Watcher monitors overdue approvals and records ReminderCount, LastReminderOn, EscalationCount, EscalatedOn, and EscalatedFlag.

## Architecture At A Glance

```text
Copilot Studio
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

## Core Components

| Area | Purpose |
|---|---|
| Copilot Studio | Conversational intake, validation, policy acknowledgement, and GetStatus status checks |
| Power Automate | Deterministic orchestration, approval routing, mock fulfilment, SLA reminders, retries, and Error capture |
| Dataverse | Dataverse as system of record for the Access Requests lifecycle |
| Teams Adaptive Card | Human-in-the-loop approval with structured decision capture |
| Azure OpenAI BYOM | CaseSummary generation as a reviewer support tool, not the decision-maker |
| Key Vault | Secure handling pattern for sensitive BYOM configuration |
| Model-driven app | Operational visibility over Status, audit history, errors, reminders, and escalation fields |
| ALM | `lai_AccessRequestAgent` solution packaging, Environment variables, Connection references, Managed solution and Unmanaged solution discipline |

## Current Status

This repo documents a portfolio case study and public GitHub foundation for the Access Request & Approval Agent. The documentation distinguishes built or demonstrated patterns from demo-only, mocked/simulated, planned, and future improvement items.

Mock fulfilment is used to demonstrate the approval-to-resolution lifecycle without granting real access. Real provisioning through Microsoft Graph, Entra ID groups, application roles, or an ITSM platform is a future improvement.

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

## Screenshot Safety

Screenshots should be redacted before publishing. Do not show tenant IDs, tenant URLs, environment URLs, private endpoint URLs, real email addresses, private approver names, client IDs, secrets, bearer tokens, connection details, live Dataverse data, or private Power Automate run history.

Use placeholders where evidence still needs to be added.
