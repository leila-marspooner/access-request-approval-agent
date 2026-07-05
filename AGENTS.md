# Agent Instructions

This is a Power Platform / Copilot Studio portfolio project for the Access Request & Approval Agent.

Use these private local context files when working in this repo:

- `source-notes/exported-chat-notes/access-request-approval-agent-context.md`
- `source-notes/exported-chat-notes/access-request-repo-plan.md`

These files are private planning material only. Do not publish `source-notes/`, do not copy raw source notes into public documentation, and do not include private screenshots or exported build notes unless they have been deliberately redacted for public use.

Keep documentation professional, recruiter-friendly, and honest. Do not overclaim production readiness. Clearly distinguish built, demonstrated, demo-only, mocked/simulated, planned, and future improvement items.

Preserve project terminology exactly, including:

- Access Request & Approval Agent
- Access Request Agent
- Access Request (Secure)
- Access Request Agent (Public)
- Access Request Agent (Internal Auth / Secure)
- `lai_AccessRequestAgent`
- Copilot Studio
- Power Automate
- Dataverse
- Teams Adaptive Card
- Model-driven app
- Entra ID
- Azure OpenAI
- Key Vault
- BYOM
- Environment variables
- Connection references
- Flow A — Create Request
- Flow H — SummariseCase
- GetStatus
- Flow B — Approval Watcher
- Flow C — Resolution Watcher
- Flow D — SLA Reminder / Escalation Watcher
- Access Requests
- CaseSummary
- CorrelationId
- RequestorEmail / UPN
- RequestorAadObjectId
- ApproverEmail
- Status
- ApprovalRequestSent
- ApprovalRequestSentOn
- ApproverDecision
- ApproverComments
- DecisionOn
- ResolvedOn
- FulfilmentStarted
- FulfilmentNotes
- ApprovalError
- FulfilmentError
- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn
- RBAC
- Least privilege
- Draft-only editing
- Field Security Profile
- Dataverse audit history
- Idempotency
- TRY/CATCH
- Error capture
- Human-in-the-loop approval
- Dataverse as system of record
- Managed solution
- Unmanaged solution
- MAJOR.MINOR.PATCH.BUILD
- Definition of Done
- Release checklist

Never include secrets, API keys, client secrets, bearer tokens, tenant IDs, tenant URLs, environment URLs, private endpoint URLs, real email addresses, private approver names, screenshots exposing secrets, live Dataverse data, connection reference secrets, private flow run data, or sensitive personal information.

This repo should make clear that the project is a portfolio/demo case study, not a production access management system. Do not claim it replaces Microsoft Entra ID Governance, a production identity governance platform, or an enterprise ITSM/access management tool.

Preserve the design principle that approval, resolution, SLA monitoring, and fulfilment logic are implemented in Power Automate rather than Agent Flows for governance, reliability, retry control, idempotency, run history, error capture, and Dataverse write-back.

Explain changes in plain English because the repo owner is learning.
