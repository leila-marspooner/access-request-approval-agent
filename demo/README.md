# Demo

This folder contains public demo guidance for the **Access Request & Approval Agent**.

The demo should show a complete governed lifecycle: conversational intake, Dataverse record creation, Teams human approval, decision write-back, simulated fulfilment, SLA monitoring, and model-driven app visibility.

## Demo Goals

- Show Copilot Studio request intake.
- Show public/demo mode and explain the secure variant.
- Show Dataverse as the system of record.
- Show Flow A — Create Request creating the Access Requests row.
- Show Flow H — SummariseCase generating CaseSummary as reviewer support.
- Show Flow B — Approval Watcher sending a Teams Adaptive Card.
- Show Flow C — Resolution Watcher processing approved/rejected outcomes.
- Show Flow D — SLA Reminder / Escalation Watcher updating reminder and escalation fields.
- Show GetStatus retrieving current state from Dataverse.
- Show the model-driven app for operational visibility.

## Demo Data

Use fictional demo data only:

- Fictional requester
- Fictional approver
- Fictional application
- Fictional justification
- Fictional comments

Do not use real email addresses, private approver names, tenant details, environment URLs, secrets, endpoint URLs, private run history, or live Dataverse records.

## What The Demo Proves

The demo proves the design pattern, not production readiness:

- Chat is the front door.
- Dataverse owns the lifecycle state.
- Power Automate performs deterministic orchestration.
- Teams captures human approval.
- AI summarisation supports review but does not decide.
- Fulfilment/provisioning is mocked or simulated.

## Related Files

- [Walkthrough](walkthrough.md)
- [Test scenarios](test-scenarios.md)
