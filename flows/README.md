# Power Automate Flows

The Access Request & Approval Agent uses a six-flow Power Automate architecture. The split is deliberate: each flow has a clear responsibility and is designed so it can be tested, evidenced, retried, and monitored separately.

## Flow List

| Flow | Role |
|---|---|
| Flow A — Create Request | Validates request input, calls SummariseCase, creates the Dataverse record |
| Flow H — SummariseCase | Generates CaseSummary through Azure OpenAI BYOM |
| GetStatus | Returns current request status from Dataverse |
| Flow B — Approval Watcher | Sends Teams Adaptive Card approval and captures the decision |
| Flow C — Resolution Watcher | Processes approved/rejected outcomes and simulated fulfilment |
| Flow D — SLA Reminder / Escalation Watcher | Monitors overdue approvals and records reminders/escalations |

## Governance Principle

Approval, resolution, SLA monitoring, and fulfilment logic are implemented in Power Automate rather than Agent Flows because they need deterministic branching, retry handling, run history, TRY/CATCH, Error capture, Idempotency, and Dataverse write-back.

## Screenshot Placeholders

Add redacted screenshots for each flow when ready. Do not expose tenant URLs, environment URLs, endpoint URLs, connection details, secrets, real emails, private run history, or live data.
