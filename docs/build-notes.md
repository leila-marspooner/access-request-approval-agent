# Build Notes

These notes summarise the implementation direction for the **Access Request & Approval Agent** portfolio case study.

They are public documentation, not private build notes. Do not add secrets, tenant values, endpoint URLs, real emails, or private implementation exports here.

## Design Summary

The project demonstrates a governed access request lifecycle:

- Copilot Studio collects the request.
- Flow A — Create Request creates the Dataverse record.
- Flow H — SummariseCase creates CaseSummary using a BYOM Azure OpenAI pattern.
- Flow B — Approval Watcher sends a Teams Adaptive Card.
- Flow C — Resolution Watcher processes the approved/rejected outcome.
- Flow D — SLA Reminder / Escalation Watcher monitors overdue approvals.
- GetStatus returns current state from Dataverse.
- The model-driven app supports operational review.

## Key Decisions

### Use Dataverse As System Of Record

Dataverse stores lifecycle state, decisions, timestamps, error fields, reminders, escalation fields, fulfilment notes, and metrics. Chat history is not treated as the record of truth.

### Keep Approval Logic Outside The Chat Layer

Power Automate handles approval, resolution, SLA monitoring, and fulfilment because those steps need deterministic control, retries, run history, idempotency, and Dataverse write-back.

### Provide Public And Secure Modes

Access Request Agent (Public) supports public portfolio review. Access Request Agent (Internal Auth / Secure) demonstrates Entra ID-backed identity capture for a more enterprise-aligned pattern.

### Use BYOM As A Child Flow

Flow H — SummariseCase keeps AI summarisation reusable and configurable. CaseSummary helps reviewers but does not decide access.

### Keep Fulfilment Mocked

Flow C — Resolution Watcher uses simulated fulfilment for the portfolio case study. Real provisioning through Microsoft Graph, Entra ID groups, app roles, or ITSM tooling is future scope.

## Current Build Status

The repo documents the architecture, flow responsibilities, Dataverse schema, governance controls, demo path, and ALM approach, supported by redacted screenshots in `screenshots/` and the architecture diagram in `architecture/`.

## Issues To Watch

- Do not overclaim production readiness.
- Do not imply real provisioning is built.
- Do not publish tenant/environment details or personal account information.
- Tie Field Security Profile, audit, RBAC, and DLP claims to evidence where available.
- Keep AI summarisation framed as reviewer support.

## Next Documentation Tasks

- Add evidence references next to relevant docs.
- Add release notes once solution package evidence is available.
