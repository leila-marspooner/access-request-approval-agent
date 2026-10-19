# Demo

This folder contains public demo guidance for the **Access Request & Approval Agent**.

The demo should show a complete governed lifecycle: conversational intake, Dataverse record creation, Teams human approval, decision write-back, simulated fulfilment, SLA monitoring, and model-driven app visibility.

## Demo Goals

- Show Copilot Studio request intake.
- Distinguish typed requester/approver emails in public/demo mode from signed-in identity gates and manager lookup in secure mode.
- Show Dataverse as the system of record.
- Show Flow A — Create Request creating the Access Requests row.
- Show Flow H — SummariseCase generating CaseSummary as reviewer support.
- Show Flow B — Approval Watcher sending a Teams Adaptive Card.
- Show Flow C — Resolution Watcher processing approved/rejected outcomes.
- Show Flow D reminder/escalation and error-scope configuration; verify field updates and failure continuation separately in a live test.
- Show allowed and generic denied GetStatus responses; explain that the exact ownership predicate remains to verify.
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

The demo illustrates the design pattern. Configuration screenshots and individual conversation/run captures do not establish that the full scenario matrix passed:

- Chat is the front door.
- Dataverse owns the lifecycle state.
- Power Automate performs deterministic orchestration.
- Teams captures human approval.
- AI summarisation supports review but does not decide.
- Fulfilment/provisioning is mocked or simulated.

## Related Files

- [Walkthrough](walkthrough.md)
- [Test scenarios](test-scenarios.md)
