# Build Notes

## Build Log

### Initial Design

The project is designed as a portfolio/demo Access Request & Approval Agent. It demonstrates a governed access request lifecycle rather than a production access management replacement.

### Agent Design

The Access Request Agent collects application, access level, justification, duration, requester identity, and approver information. Public/demo mode is low-friction. Secure mode demonstrates Entra ID identity capture.

### Flow Design

The solution uses a six-flow architecture:

- Flow A — Create Request
- Flow H — SummariseCase
- GetStatus
- Flow B — Approval Watcher
- Flow C — Resolution Watcher
- Flow D — SLA Reminder / Escalation Watcher

### Governance Design

Governance patterns include Dataverse as system of record, RBAC, Least privilege, Draft-only editing, Field Security Profile, Dataverse audit history, Idempotency, TRY/CATCH, Error capture, CorrelationId, Environment variables, Connection references, and ALM release checks.

## Key Decisions

### Use Dataverse As System Of Record

Dataverse stores lifecycle state, approval decisions, timestamps, error fields, reminders, escalation fields, and fulfilment notes. The process does not rely on chat history as the record of truth.

### Keep Approval Logic Outside The Chat Layer

Power Automate handles approval, resolution, SLA monitoring, and fulfilment because those steps need deterministic control, retries, run history, Idempotency, and Dataverse write-back.

### Provide Public And Secure Modes

Access Request Agent (Public) supports public portfolio review. Access Request Agent (Internal Auth / Secure) demonstrates a stronger enterprise pattern using Entra ID identity capture.

### Use BYOM As A Child Flow

Flow H — SummariseCase keeps Azure OpenAI summarisation reusable and configurable. The CaseSummary helps reviewers but does not decide access.

### Keep Fulfilment Mocked

Flow C — Resolution Watcher uses simulated fulfilment for the portfolio case study. Real provisioning through Graph, Entra ID groups, application roles, or ITSM tooling is a future improvement.

## Issues To Watch

- Screenshots must be redacted before publishing.
- Public docs must not include tenant IDs, tenant URLs, environment URLs, real emails, client secrets, endpoint URLs, bearer tokens, connection details, or private run history.
- Field security and audit claims should be tied to screenshot or implementation evidence before being described as fully implemented.
- ALM materials should be described as portfolio release discipline, not proof of production deployment.

## Next Steps

- Add redacted screenshots.
- Add architecture diagram.
- Add implementation evidence references where safe.
- Review public docs for terminology consistency.
- Prepare first GitHub commit after user approval.
