# Architecture Assets

This folder is for public-safe architecture diagrams and exports for the Access Request & Approval Agent.

## Planned Diagram

Recommended file:

- `access-request-architecture.png`

The diagram should show:

- Copilot Studio Access Request Agent
- Access Request Agent (Public)
- Access Request Agent (Internal Auth / Secure)
- Entra ID identity capture for the secure variant
- Flow A — Create Request
- Flow H — SummariseCase
- GetStatus
- Flow B — Approval Watcher
- Flow C — Resolution Watcher
- Flow D — SLA Reminder / Escalation Watcher
- Dataverse Access Requests table
- Teams Adaptive Card approval
- Azure OpenAI BYOM
- Key Vault
- Environment variables
- Connection references
- Model-driven app

## Redaction Rules

Do not include tenant IDs, tenant URLs, environment URLs, private endpoint URLs, real email addresses, secrets, client IDs, private environment names, or live Dataverse data in diagrams.

Use generic labels such as "Approver", "Requester", "Secure environment", and "Azure OpenAI endpoint" rather than real tenant-specific values.
