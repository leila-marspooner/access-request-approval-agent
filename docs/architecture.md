# Architecture

## High-Level Design

The Access Request & Approval Agent uses Copilot Studio as the user-facing entry point and Power Automate as the deterministic orchestration layer. Dataverse stores the authoritative Access Requests record, Teams Adaptive Cards capture Human-in-the-loop approval, Azure OpenAI BYOM generates a CaseSummary, and a Model-driven app gives operations users visibility into the lifecycle.

```text
Requester
  -> Copilot Studio Access Request Agent
  -> Flow A — Create Request
  -> Flow H — SummariseCase
  -> Dataverse Access Requests
  -> Flow B — Approval Watcher
  -> Teams Adaptive Card approver decision
  -> Dataverse decision update
  -> Flow C — Resolution Watcher
  -> Flow D — SLA Reminder / Escalation Watcher
  -> Model-driven app operational visibility
```

## Why Power Automate Instead Of Agent Flows

Approval, resolution, SLA monitoring, and fulfilment logic are implemented in Power Automate rather than Agent Flows. These lifecycle steps need deterministic branching, Idempotency, retry handling, run history, TRY/CATCH, Error capture, and reliable Dataverse write-back.

The agent stays focused on conversational intake, validation, policy acknowledgement, and status lookup.

## Public And Secure Modes

### Public/Demo Mode

Access Request Agent (Public) is designed for low-friction demonstration. It can collect RequestorEmail / UPN conversationally so a reviewer can see the flow without tenant sign-in.

### Secure/Enterprise Mode

Access Request Agent (Internal Auth / Secure) and Access Request (Secure) demonstrate Entra ID identity capture. In this mode, RequestorEmail / UPN and RequestorAadObjectId are captured from authenticated context where available instead of relying on a user-typed value.

## Dataverse Layer

Dataverse is the system of record. The Access Requests table stores request details, Status, approval state, timestamps, CaseSummary, CorrelationId, error fields, reminder counters, escalation fields, and fulfilment notes.

Dataverse is not passive storage in this design. It is the lifecycle authority that watcher flows read from and write back to.

## AI Summarisation Layer

Flow H — SummariseCase uses an Azure OpenAI BYOM pattern to create CaseSummary. The summary helps the approver review context quickly, but it does not approve, reject, risk-score, or make access decisions.

Configuration is driven by Environment variables and Key Vault-backed secret handling where applicable. If the model call fails, the process should return a deterministic fallback summary so Flow A — Create Request can continue.

## Operational Visibility

The Model-driven app gives approvers, fulfilment/operations users, and admins a structured view of Access Requests records, Status, decision fields, CaseSummary, Error capture, reminders, escalation state, and Dataverse audit history.

## Architecture Diagram Placeholder

Add a public-safe architecture diagram here when ready:

- `architecture/access-request-architecture.png`

The diagram should show Copilot Studio, Power Automate flows, Dataverse, Teams Adaptive Card approval, Entra ID, Azure OpenAI, Key Vault, Environment variables, Connection references, and the Model-driven app. Do not include tenant URLs, environment URLs, endpoint URLs, email addresses, IDs, secrets, or private run data.
