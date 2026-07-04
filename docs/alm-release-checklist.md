# ALM Release Checklist

This checklist documents release discipline for the Access Request & Approval Agent portfolio project. It is not proof of production deployment.

## Solution Packaging

- Confirm solution name: `lai_AccessRequestAgent`
- Confirm publisher prefix: `lai`
- Confirm all required components are included in the solution
- Export an Unmanaged solution for development backup
- Export a Managed solution for target environment deployment when appropriate
- Use MAJOR.MINOR.PATCH.BUILD versioning

## Environment Variables

Confirm Environment variables are configured per environment and do not hardcode secrets or tenant-specific values:

- `lai_BYOM_Endpoint`
- `lai_BYOM_DeploymentName`
- `lai_BYOM_ApiVersion`
- `lai_BYOM_ApiKey`
- `lai_BYOM_SystemPrompt`
- `lai_SLA_ReminderHours`
- `lai_SLA_MaxReminders`
- `lai_EscalationRecipient`
- `lai_ApprovalChannelOrMailbox`

## Connection References

- Confirm Power Automate flows use Connection references.
- Confirm connections are set after import.
- Do not export or publish connection secrets.
- Do not include private connection details in screenshots.

## Security Checks

- Confirm RBAC roles are documented.
- Confirm Least privilege intent is reviewed.
- Confirm Field Security Profile evidence is redacted before publishing.
- Confirm Dataverse audit history is enabled where evidenced.
- Confirm Draft-only editing behaviour is documented or evidenced.

## Flow Checks

- Flow A — Create Request validates inputs and writes CorrelationId.
- Flow H — SummariseCase has fallback handling.
- GetStatus reads from Dataverse.
- Flow B — Approval Watcher uses ApprovalRequestSent for Idempotency.
- Flow C — Resolution Watcher prevents duplicate mock fulfilment.
- Flow D — SLA Reminder / Escalation Watcher increments reminder and escalation fields.
- TRY/CATCH and Error capture patterns are documented.

## Screenshot Review

- Remove tenant IDs, URLs, environment names, client IDs, secrets, tokens, emails, approver names, and private run history.
- Use placeholder images where evidence is not ready.
- Confirm no live Dataverse data is visible.

## Definition Of Done

- README is accurate and public-safe.
- All core docs are present.
- source-notes/ is ignored.
- No secrets or private data are committed.
- Screenshot placeholders or redacted screenshots are included.
- Fulfilment is clearly labelled as mocked/simulated.
- AI summarisation is described as reviewer support, not a decision-maker.
- Release checklist is complete for portfolio readiness.
