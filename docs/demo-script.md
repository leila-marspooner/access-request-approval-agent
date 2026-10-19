# Demo Script

## Demo Goal

Show that the **Access Request & Approval Agent** is a governed request lifecycle, not only a chatbot.

The demo should show conversational intake, Dataverse tracking, Teams approval, BYOM CaseSummary generation, simulated fulfilment, SLA monitoring, and model-driven app visibility.

## Scene 1 — Request Intake

Show the requester using Copilot Studio to request access.

Suggested fictional values:

- ApplicationName: Jira or ServiceNow
- AccessLevel: Admin or Elevated
- DurationDays: 14
- Justification: temporary project need
- Public/demo only: fictional requester and approver email inputs

For secure mode, show the identity gates before intake and explain that requester email comes from sign-in. Principal name is passed into the existing ApproverEmail flow input for the manager lookup path; the requester does not type a manager address. Do not claim object-ID capture from this six-input call.

Highlight validation, submission confirmation and the Admin access acknowledgement gate where applicable.

## Scene 2 — Request Created

Show Flow A — Create Request manager lookup and row creation. Compare the successful pictured actions with lookup NotFound entering CATCH; distinguish this from the untested manager-email-empty branch. Expanded lookup/row mappings still need verification.

Explain that Dataverse becomes the system of record after submission and that the agent does not own the lifecycle state.

## Scene 3 — BYOM Summary

Show Flow H — SummariseCase returning CaseSummary.

Explain that Azure OpenAI BYOM supports the reviewer but does not make approval decisions. Mention environment variables and Key Vault-backed secret handling as the safer configuration pattern where implemented.

## Scene 4 — Teams Approval

Show Flow B — Approval Watcher sending the Teams Adaptive Card.

The approver reviews the request, adds comments, and approves or rejects. Then show ApproverDecision, ApproverComments, DecisionOn, Status, and Status Reason written back to Dataverse.

## Scene 5 — Status Check

Show the requester asking the agent for status.

Show the [allowed and generic denied responses](../flows/flow-get-status.md#observed-status-responses). They are Dataverse-backed status examples; exact ownership enforcement and the identities in these sessions remain unverified. Displayed Status `Active` is not proof of real access provisioning.

## Scene 6 — Resolution / Mock Fulfilment

Show Flow C — Resolution Watcher processing the outcome.

For an approved request, explain that fulfilment/provisioning is mocked or simulated. Show ResolvedOn, FulfilmentStarted, and FulfilmentNotes where appropriate.

For a rejected request, show that the lifecycle closes without fulfilment.

## Scene 7 — SLA Watcher

Show Flow D — SLA Reminder / Escalation Watcher configuration for overdue pending approvals, including per-request and outer error scopes. Present the following fields as the intended write-back design. Runtime delivery, exact updates and continuation after item failure remain to verify; do not narrate the configuration as a passing sweep test.

Highlight:

- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn

## Scene 8 — Governance And ALM

Show the model-driven app, RBAC/security evidence, Field Security Profile evidence where implemented, Dataverse audit history, environment variables, connection references, and release checklist.

## Closing Line

This project demonstrates how a Copilot Studio agent can act as the front door to a governed business-process pattern while Power Automate, Dataverse, Teams, Entra ID, Azure OpenAI, Key Vault, RBAC, audit history, SLA monitoring, and ALM controls provide the architecture behind it.
