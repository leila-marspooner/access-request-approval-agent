# Architecture

This folder is for public-safe architecture diagrams and architecture notes for the **Access Request & Approval Agent**.

The project uses Copilot Studio for conversational intake, Power Automate for deterministic orchestration, Dataverse as the system of record, Teams Adaptive Cards for human approval, and a model-driven app for operational visibility.

## Request Lifecycle

```text
Requester
  -> Copilot Studio Access Request Agent
  -> Flow A — Create Request
  -> Flow H — SummariseCase
  -> Dataverse Access Requests
  -> Flow B — Approval Watcher
  -> Teams Adaptive Card
  -> Dataverse decision write-back
  -> Flow C — Resolution Watcher
  -> Flow D — SLA Reminder / Escalation Watcher
  -> Model-driven app visibility
```

## Component Responsibilities

| Component | Responsibility |
|---|---|
| Copilot Studio | Conversational intake, validation, admin-access acknowledgement, and status checks |
| Flow A — Create Request | Creates the Access Requests record and starts the lifecycle |
| Flow H — SummariseCase | Generates CaseSummary through a BYOM Azure OpenAI pattern with fallback handling |
| GetStatus | Returns current lifecycle state from Dataverse |
| Flow B — Approval Watcher | Sends a Teams Adaptive Card and writes the decision back to Dataverse |
| Flow C — Resolution Watcher | Processes approved/rejected outcomes and performs simulated fulfilment where appropriate |
| Flow D — SLA Reminder / Escalation Watcher | Monitors overdue approvals and records reminder/escalation state |
| Dataverse | Stores authoritative state, decisions, timestamps, errors, metrics, and audit history |
| Model-driven app | Gives approvers, operations users, and admins a governed operational view |

## Why The Design Is Modular

The lifecycle is split across focused flows so each part can be tested, evidenced, retried, monitored, and explained separately.

This is especially important for approval and SLA processes, where duplicate messages, hidden failures, or unclear status can undermine trust.

## Public And Secure Architecture

| Mode | Purpose | Identity Pattern |
|---|---|---|
| Access Request Agent (Public) | Easy portfolio review and demo testing | RequestorEmail / UPN may be typed by the requester |
| Access Request Agent (Internal Auth / Secure) | Enterprise-aligned intake pattern | RequestorEmail / UPN and RequestorAadObjectId are captured from Entra ID context where available |

## Planned Diagram

Recommended file:

- `access-request-architecture.png`

The diagram should show:

- Access Request Agent (Public)
- Access Request Agent (Internal Auth / Secure)
- Entra ID identity capture
- Flow A — Create Request
- Flow H — SummariseCase
- GetStatus
- Flow B — Approval Watcher
- Flow C — Resolution Watcher
- Flow D — SLA Reminder / Escalation Watcher
- Access Requests Dataverse table
- Teams Adaptive Card approval
- Azure OpenAI BYOM
- Key Vault pattern for sensitive configuration
- Environment variables
- Connection references
- Model-driven app

## Future Production Architecture Improvements

Future hardening could include Microsoft Graph provisioning, Entra group or app-role assignment, automated approver validation, DLP policy evidence, Managed Environment evidence, centralized monitoring, and deployment pipeline automation.

Do not describe those items as built unless implementation evidence is added.

## Redaction Rules

Architecture diagrams must not include tenant IDs, tenant URLs, environment URLs, private endpoint URLs, real email addresses, client IDs, app registration IDs, secrets, private environment names, or live Dataverse data.
