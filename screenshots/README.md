# Screenshots

This folder is for public-safe screenshots only.

Screenshots are evidence for the portfolio case study. They should show the agent, flows, Dataverse, Teams Adaptive Cards, model-driven app, security controls, and ALM artefacts without exposing private tenant, account, endpoint, or configuration details.

## Suggested Subfolders

```text
screenshots/
├── agent/
├── flows/
├── dataverse/
├── teams/
├── governance/
├── alm/
└── demo/
```

## Naming Convention

Use descriptive, stable filenames:

- `agent-request-access-topic.png`
- `flow-a-create-request-overview.png`
- `teams-approval-card-redacted.png`
- `dataverse-access-request-record.png`
- `model-driven-app-sla-view.png`
- `alm-connection-references.png`

## Required Screenshot Set

| Screenshot | What It Proves |
|---|---|
| Copilot Studio request intake | Conversational entry point and structured data capture |
| Admin access policy acknowledgement | Higher-risk request guardrail |
| Access Request (Secure) identity evidence | Entra ID-backed identity pattern after redaction |
| Flow A — Create Request | Request creation and Dataverse write-back |
| Flow H — SummariseCase | BYOM CaseSummary support flow |
| GetStatus | Dataverse-backed status lookup |
| Flow B — Approval Watcher | Teams Adaptive Card approval orchestration |
| Flow C — Resolution Watcher | Approved/rejected lifecycle processing and simulated fulfilment |
| Flow D — SLA Reminder / Escalation Watcher | Reminder and escalation monitoring |
| Access Requests row | Dataverse as system of record |
| Model-driven app views | Operational visibility |
| RBAC/security role evidence | Least privilege design |
| Field Security Profile evidence | Internal field protection where implemented |
| Dataverse audit history | Traceability of lifecycle changes |
| Environment variables | Configuration externalisation |
| Connection references | ALM-friendly connector abstraction |
| Managed/unmanaged solution evidence | Release discipline |

## Redaction Rules

Before publishing, blur or crop:

- Tenant IDs, tenant names, tenant URLs, and environment URLs
- Private environment names
- Browser address bars containing tenant or environment details
- Real email addresses and personal account details
- Private approver names
- Client IDs, app registration IDs, subscription IDs, and environment IDs
- API keys, client secrets, bearer tokens, passwords, and HTTP authentication headers
- Azure OpenAI endpoint URLs and Key Vault secret values
- Connection names tied to personal accounts
- Power Automate run inputs/outputs containing private configuration
- Browser tabs, bookmarks, downloads, or profile details if private

Do not blur fictional demo request data unless it exposes real account or tenant information.

## README Image Recommendations

Use only the strongest, cleanest screenshots in the root README:

- One Copilot Studio intake screenshot
- One architecture or lifecycle diagram
- One Teams approval screenshot
- One model-driven app or Dataverse evidence screenshot

Keep denser evidence, such as audit history and full flow canvases, in the relevant documentation pages.

## Publishing Rule

Do not commit unredacted screenshots. Use placeholders until safe screenshots are ready.
