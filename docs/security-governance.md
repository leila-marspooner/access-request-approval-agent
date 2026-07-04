# Security And Governance

## Security Positioning

The Access Request & Approval Agent demonstrates enterprise-aligned governance patterns in a portfolio/demo environment. It should not be described as a production access management system without further hardening, review, monitoring, and real provisioning controls.

## Identity

Access Request Agent (Public) supports demo access and may collect RequestorEmail / UPN conversationally.

Access Request Agent (Internal Auth / Secure) and Access Request (Secure) demonstrate Entra ID identity capture. In the secure variant, RequestorEmail / UPN and RequestorAadObjectId are captured from authenticated context where available.

## RBAC And Least Privilege

Recommended roles:

| Role | Intent |
|---|---|
| Access Request - Requester | Create requests and read own records |
| Access Request - Approver | Read assigned/pending requests and update approval decision fields |
| Access Request - Fulfilment/Ops | Read/write operational and fulfilment fields |
| Access Request - Admin | Manage the model-driven app, request records, and operational configuration |
| Access Request - Automation Service | Optional service role for flows and automation |

The public docs should describe RBAC as evidenced only where screenshots or implementation proof exist.

## Field Security Profile

Field Security Profile controls are intended for internal/system fields such as RequestorAadObjectId, CorrelationId, ApprovalError, FulfillmentError, FulfillmentNotes, and CaseSummary where appropriate.

Only claim field security as implemented where evidence exists. Otherwise, describe it as a documented governance control or hardening item.

## Dataverse Audit History

Dataverse audit history supports traceability for changes to Status, ApproverDecision, ApproverComments, DecisionOn, FulfilledOn, ReminderCount, LastReminderOn, EscalationCount, EscalatedOn, ApprovalError, and FulfillmentError.

Screenshots of audit history must be redacted before publishing.

## Idempotency

Approval Idempotency:

- ApprovalRequestSent prevents duplicate Teams Adaptive Cards.
- ApprovalRequestSentOn records when the approval card was sent.

Fulfilment Idempotency:

- FulfillmentStarted and/or FulfilledOn prevents duplicate simulated fulfilment.

SLA Idempotency:

- ReminderCount and LastReminderOn reduce reminder spam.
- EscalationCount, EscalatedOn, and EscalatedFlag track escalation state.

## Error Capture

TRY/CATCH patterns should write concise operational errors back to Dataverse:

- ApprovalError for approval watcher failures
- FulfillmentError for resolution or mock fulfilment failures

The goal is to make operational issues visible in the Model-driven app without requiring every reviewer to inspect Power Automate run history.

## AI Governance

Flow H — SummariseCase uses Azure OpenAI BYOM to generate CaseSummary. The AI summary supports the approver but does not make access decisions.

BYOM configuration should be driven through Environment variables and Key Vault-backed secret handling. Do not hardcode or publish endpoints, API keys, prompts containing sensitive data, tenant values, or private connection details.

## Public Safety Rules

Never publish:

- Secrets, API keys, client secrets, bearer tokens, or passwords
- Tenant IDs, tenant URLs, environment URLs, endpoint URLs, or private environment names
- Real email addresses, private approver names, personal data, or live Dataverse data
- Connection details, connection secrets, or private flow run data
- Screenshots exposing sensitive configuration
