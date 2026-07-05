# ALM Release Checklist

This checklist documents release discipline for the **Access Request & Approval Agent** portfolio project.

It is not proof of production deployment. It shows the ALM thinking that would be expected before moving a Power Platform solution between environments.

## Solution Identity

| Item | Value |
|---|---|
| Solution name | `lai_AccessRequestAgent` |
| Publisher prefix | `lai` |
| Versioning pattern | MAJOR.MINOR.PATCH.BUILD |

## Managed And Unmanaged Solutions

- Use the unmanaged solution for development.
- Export a managed solution for UAT or production-style import.
- Retain previous managed solution packages as rollback artefacts.
- Do not make direct hotfixes in UAT or production-style environments.
- Fix in development, increment version, export, and redeploy.

## Environment Variables

Confirm environment-specific values are externalised and not hardcoded in flow logic.

Recommended variables include:

- BYOM endpoint
- BYOM deployment name
- BYOM API version
- BYOM system prompt
- SLA reminder threshold
- Maximum reminder count
- Escalation recipient
- Approval routing destination

API keys, client secrets, bearer tokens, endpoint URLs, tenant URLs, and private recipient values must not be committed or shown in public screenshots.

## Connection References

- Confirm Power Automate flows use connection references.
- Confirm connections are rebound after import.
- Prefer service-owned or environment-appropriate connections for UAT/production-style deployments.
- Do not publish personal connection details or connector secrets.

## Security Checks

- Confirm RBAC roles are documented.
- Confirm least privilege intent is reviewed.
- Confirm Field Security Profile evidence is redacted before publishing.
- Confirm Dataverse audit history evidence is redacted before publishing.
- Confirm draft-only editing behaviour is documented or evidenced.
- Confirm public/demo mode is not described as a secure production pattern.

## Flow Checks

| Flow | Release Check |
|---|---|
| Flow A — Create Request | Validates inputs, generates CorrelationId, creates Access Requests row |
| Flow H — SummariseCase | Uses environment-driven BYOM configuration and fallback handling |
| GetStatus | Reads current lifecycle state from Dataverse |
| Flow B — Approval Watcher | Uses ApprovalRequestSent and ApprovalRequestSentOn to help prevent duplicate cards |
| Flow C — Resolution Watcher | Uses ResolvedOn and FulfilmentStarted to help prevent duplicate resolution work |
| Flow D — SLA Reminder / Escalation Watcher | Updates ReminderCount, LastReminderOn, Escalated, EscalationCount, and EscalatedOn |

## Screenshot Checks

Before publishing screenshots, remove or blur:

- Tenant IDs, tenant names, tenant URLs, and environment URLs
- Private environment names
- Client IDs, app registration IDs, subscription IDs, and environment IDs
- API keys, client secrets, bearer tokens, endpoint values, and Key Vault secret values
- Real email addresses, personal account details, and private approver names
- Private Power Automate run inputs/outputs
- Connection details and personal maker connections

## Pre-release Checks

- README is accurate and public-safe.
- Core docs are present and consistent.
- Private local notes remain ignored.
- No secrets, private notes, or unredacted screenshots are staged.
- Fulfilment/provisioning is clearly described as mocked or simulated.
- AI summarisation is described as reviewer support, not the decision-maker.

## Post-import Checks

- Connection references are bound.
- Environment variables are populated.
- Flows are enabled intentionally.
- Security roles are assigned to the correct test users or teams.
- Test request can be created.
- Flow B — Approval Watcher sends only one Teams Adaptive Card.
- Model-driven app shows the expected lifecycle fields.

## Rollback Considerations

- Keep previous managed solution exports.
- Record the version being imported.
- Keep release notes for meaningful changes.
- Avoid deleting data or components without a rollback plan.
