# Demo Script

## Demo Goal

Show that the Access Request & Approval Agent is more than a chatbot. It is a governed request-to-approval lifecycle with conversational intake, Dataverse tracking, Teams approval, AI-assisted CaseSummary, simulated fulfilment, SLA monitoring, and operational visibility.

## Scene 1 — Request Intake

Show the requester using Copilot Studio to ask for access.

Suggested demo values:

- Application: Azure DevOps or another non-sensitive demo application
- Access level: Admin
- Duration: 14 days
- Justification: temporary project need
- Approver: redacted demo approver address

Highlight the structured questions, validation, and Admin access policy acknowledgement gate.

## Scene 2 — Request Created

Show Flow A — Create Request returning a request reference, Status, CaseSummary, and CorrelationId.

Explain that Dataverse is the system of record and that the agent does not own the lifecycle after submission.

## Scene 3 — BYOM Summary

Show Flow H — SummariseCase.

Explain that Azure OpenAI BYOM creates CaseSummary as a support tool for the reviewer. It does not make the approval decision. Mention that Key Vault and Environment variables keep configuration out of flow logic.

## Scene 4 — Teams Approval

Show the Teams Adaptive Card sent by Flow B — Approval Watcher.

The approver reviews the request, adds comments, and approves or rejects. Then show Dataverse updated with ApproverDecision, ApproverComments, DecisionOn, and Status.

## Scene 5 — Status Check

Show the user asking the Access Request Agent for status. The GetStatus flow retrieves the current Status and key timestamps from Dataverse.

## Scene 6 — Resolution / Mock Fulfilment

Show Flow C — Resolution Watcher processing the outcome. Make clear that fulfilment/provisioning is mocked or simulated unless real provisioning evidence is added later.

## Scene 7 — SLA Watcher

Show Flow D — SLA Reminder / Escalation Watcher checking overdue pending approvals. Highlight ReminderCount, LastReminderOn, EscalationCount, EscalatedOn, and EscalatedFlag.

## Scene 8 — Governance And ALM

Show the Model-driven app, RBAC/security evidence, Field Security Profile where evidenced, Dataverse audit history, Environment variables, Connection references, and Release checklist.

## Closing Line

This project demonstrates how a conversational agent can act as the front door to a governed business process while Power Automate, Dataverse, Teams, Entra ID, Azure OpenAI, Key Vault, RBAC, audit history, SLA monitoring, and ALM controls provide the enterprise architecture behind it.
