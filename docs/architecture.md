# Architecture

The **Access Request & Approval Agent** demonstrates a governed Power Platform architecture for access request intake and approval.

Copilot Studio provides the conversational front end. Power Automate handles deterministic lifecycle orchestration. Dataverse stores the authoritative record. Teams Adaptive Cards capture human approval. Azure OpenAI BYOM generates CaseSummary as reviewer support. The model-driven app gives operational visibility.

## High-Level Flow

```text
Requester
  -> Copilot Studio Access Request Agent
  -> Flow A — Create Request
  -> Flow H — SummariseCase
  -> Dataverse Access Requests
  -> Flow B — Approval Watcher
  -> Teams Adaptive Card
  -> Dataverse decision update
  -> Flow C — Resolution Watcher
  -> Flow D — SLA Reminder / Escalation Watcher
  -> Model-driven app
```

## Why Power Automate Instead Of Agent Flows

Approval, resolution, SLA monitoring, and fulfilment logic are implemented in Power Automate because these lifecycle steps need:

- Deterministic branching
- Retry control
- Run history
- Idempotency
- TRY/CATCH-style error capture
- Dataverse write-back
- Operational supportability

The agent stays focused on intake, validation, acknowledgement, and status lookup.

## Dataverse-Centred Design

The Access Requests table is the system of record. It stores request details, Status, Status Reason, approval fields, CaseSummary, CorrelationId, ResolvedOn, fulfilment fields, reminder fields, escalation fields, error fields, and metrics.

Watcher flows use Dataverse state to decide what should happen next. This makes the lifecycle inspectable through the model-driven app rather than hidden in chat history.

## Public And Secure Modes

| Mode | Purpose | Identity Pattern |
|---|---|---|
| Access Request Agent (Public) | Low-friction public portfolio demo | RequestorEmail / UPN may be collected conversationally |
| Access Request Agent (Internal Auth / Secure) | Enterprise-aligned authenticated pattern | RequestorEmail / UPN and RequestorAadObjectId are captured from Entra ID context where available |

## AI Summarisation Layer

Flow H — SummariseCase uses a BYOM Azure OpenAI pattern to generate CaseSummary.

The summary helps the approver review context. It does not approve, reject, risk-score, or provision access. If the AI call fails, the request should continue with a deterministic fallback summary.

## Operational Visibility

The model-driven app surfaces:

- Current request status
- Approval decisions and comments
- CaseSummary and CorrelationId
- Mock fulfilment and resolution state
- SLA reminder and escalation fields
- Error capture
- Dataverse audit history
- Operational metrics

## Diagram Placeholder

Add a public-safe architecture diagram when ready:

- `architecture/access-request-architecture.png`

The diagram should not include tenant URLs, environment URLs, endpoint URLs, email addresses, IDs, secrets, or private run data.
