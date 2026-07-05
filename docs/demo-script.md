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
- ApproverEmail: redacted demo approver address

Highlight validation and the Admin access acknowledgement gate where applicable.

## Scene 2 — Request Created

Show Flow A — Create Request creating the Access Requests row.

Explain that Dataverse becomes the system of record after submission and that the agent does not own the lifecycle state.

## Scene 3 — BYOM Summary

Show Flow H — SummariseCase returning CaseSummary.

Explain that Azure OpenAI BYOM supports the reviewer but does not make approval decisions. Mention environment variables and Key Vault-backed secret handling as the safer configuration pattern where implemented.

## Scene 4 — Teams Approval

Show Flow B — Approval Watcher sending the Teams Adaptive Card.

The approver reviews the request, adds comments, and approves or rejects. Then show ApproverDecision, ApproverComments, DecisionOn, Status, and Status Reason written back to Dataverse.

## Scene 5 — Status Check

Show the requester asking the agent for status.

GetStatus should retrieve current lifecycle state from Dataverse rather than relying on chat memory.

## Scene 6 — Resolution / Mock Fulfilment

Show Flow C — Resolution Watcher processing the outcome.

For an approved request, explain that fulfilment/provisioning is mocked or simulated. Show ResolvedOn, FulfilmentStarted, and FulfilmentNotes where appropriate.

For a rejected request, show that the lifecycle closes without fulfilment.

## Scene 7 — SLA Watcher

Show Flow D — SLA Reminder / Escalation Watcher checking overdue pending approvals.

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
