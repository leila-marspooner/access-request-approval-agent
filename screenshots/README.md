# Screenshots

This folder is for public-safe screenshots only.

Screenshots are evidence for the portfolio case study. They should show the agent, flows, Dataverse, Teams Adaptive Cards, model-driven app, security controls, and ALM artefacts without exposing private tenant, account, endpoint, or configuration details.

## Organisation and Naming

Screenshots are stored in this folder with descriptive camelCase filenames grouped by prefix, for example:

- `copilotTopic...` — Copilot Studio topic and agent evidence
- `createFlow...`, `approvalWatcher...`, `resolutionWatcher...`, `sla...`, `getStatus...` — Power Automate flow evidence
- `byom...` — Azure OpenAI BYOM CaseSummary configuration
- `accessRequestsTable...`, `auditHistory...`, `fieldLevelSecurity...` — Dataverse and audit evidence
- `modelDriven...`, `modelApp...` — model-driven app views
- `securityRole...`, `accessCarousel...` — RBAC and governance evidence
- `teams...`, `email-...` — Teams approval and notification evidence
- `environmentVariables`, `connectionReferences`, `solutionOverview` — ALM evidence

## Evidence Areas

| Evidence area | What the screenshots demonstrate |
|---|---|
| Copilot Studio request intake | Conversational entry point and structured data capture |
| Admin access policy acknowledgement | Higher-risk request guardrail |
| Access Request (Secure) identity evidence | Signed-in topic gates and a refusal response; claim binding still needs controlled tests |
| Flow A — Create Request | Request creation and Dataverse write-back |
| Flow H — SummariseCase | BYOM CaseSummary support flow |
| GetStatus | Allowed and generic denied responses; underlying ownership predicate remains to verify |
| Flow B — Approval Watcher | Teams Adaptive Card approval orchestration |
| Flow C — Resolution Watcher | Approved/rejected lifecycle processing and simulated fulfilment |
| Flow D — SLA Reminder / Escalation Watcher | Reminder/escalation and error-scope configuration; current runtime continuation unverified |
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

Do not commit unredacted screenshots. All screenshots in this folder must pass the redaction rules above before publishing.

## Evidence Index

Publication copies were reviewed visually. Source captures remain local and ignored; earlier replaced publication images were preserved privately. Publication files are re-encoded without source metadata. The identity-refusal copy masks the sign-in code; the full request topic crops maker-profile chrome. No source capture is intended for Git publication.

| Publication image | Evidence and mapping |
|---|---|
| [createFlow1.png](createFlow1.png) | Create Request canvas; manager lookup and create-row branch. |
| [createFlowRunHistorySuccess.png](createFlowRunHistorySuccess.png) | Create Request run view; pictured lookup and creation actions succeeded. |
| [createFlowRunSuccessReturn.png](createFlowRunSuccessReturn.png) | Successful-run continuation; CATCH skipped, FINALLY return succeeded. |
| [createFlowManagerNotFound.png](createFlowManagerNotFound.png) | Lookup NotFound run; CATCH executes and creation is skipped. |
| [createFlowCatchFinally.png](createFlowCatchFinally.png) | CATCH/FINALLY configuration; values and run-after settings collapsed. |
| [slaWatcherOverview.png](slaWatcherOverview.png) | Recurrence, per-request handler and outer CATCH configuration. |
| [slaMaxRemindersGate.png](slaMaxRemindersGate.png) | Reminder-gate canvas; reminder/escalation branches, no runtime proof. |
| [copilotStatusOwnershipDenied.png](copilotStatusOwnershipDenied.png) | Generic permission denial for AR001025; identity/predicate not shown. |
| [copilotStatusNotFound.png](copilotStatusNotFound.png) | Generic denial for AR000000 and retry prompt. |
| [copilotStatusAllowed.png](copilotStatusAllowed.png) | AR001025 details and summary; displayed Active does not mean access granted. |
| [copilotRequestIdentityDenied.png](copilotRequestIdentityDenied.png) | Identity refusal conversation; sign-in code masked. |
| [copilotTopicRequestAccess1.png](copilotTopicRequestAccess1.png) | Request Access topic; signed-in gates, validation and flow mappings. Maker chrome cropped. |

The existing GetStatus flow/topic canvases, earlier intake views, SLA failed-TRY/successful-CATCH run, Teams, BYOM, model-driven app, Dataverse, security and ALM screenshots remain useful evidence. Earlier intake images are configuration views, not independent test-pass evidence. The status conversations supplement the older GetStatus canvas; they do not establish its current authorization configuration.

The six-flow main architecture remains the case study architecture. Detailed limits and outstanding tests are maintained on [Flow A](../flows/flow-a-create-request.md#evidence-limits-and-outstanding-tests), [GetStatus](../flows/flow-get-status.md#ownership-restrictions-and-verification) and [Flow D](../flows/flow-d-sla-watcher.md#evidence-limits-and-outstanding-tests).
