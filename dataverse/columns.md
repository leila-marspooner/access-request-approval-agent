# Columns

This file documents the public schema narrative for the **Access Requests** table.

Logical names should be verified against the final Dataverse solution before publishing generated schema exports. Do not publish schema exports that include tenant-specific IDs, environment URLs, private connection details, or live data.

## Request Reference

| Field | Purpose |
|---|---|
| Request Number | User-facing request reference |
| RequestName | Human-readable request name used in forms and views |
| Request Title (Primary) | Primary column for the Dataverse row |
| CorrelationId | Trace identifier across Copilot Studio, Power Automate, Dataverse, Teams, and the model-driven app |

## Requester And Approver

| Field | Purpose |
|---|---|
| RequestorEmail | Requester email or UPN |
| RequestorAadObjectId | Entra ID object ID in the secure variant where captured |
| ApproverEmail | Approver address used for routing the approval |

## Request Details

| Field | Purpose |
|---|---|
| ApplicationName | Requested application or business system |
| AccessLevel | Requested access level, such as Standard, Elevated, or Admin |
| Justification | Business reason for access |
| DurationDays | Requested access duration |

## Lifecycle

| Field | Purpose |
|---|---|
| Status | Current lifecycle state |
| Status Reason | More specific lifecycle reason where used |
| SubmittedOn | Request submission timestamp |
| DecisionOn | Approval decision timestamp |
| ResolvedOn | Resolution timestamp after approved/rejected outcome processing |

## Approval

| Field | Purpose |
|---|---|
| ApprovalRequestSent | Idempotency flag showing an approval card has been sent |
| ApprovalRequestSentOn | Timestamp for the approval card send |
| ApproverDecision | Human decision captured from the Teams Adaptive Card |
| ApproverComments | Human comments captured with the decision |
| ApprovalError | Flow B — Approval Watcher error capture |

## AI Summary

| Field | Purpose |
|---|---|
| CaseSummary | BYOM Azure OpenAI summary for reviewer support |

CaseSummary supports the approver. It must not be treated as the decision-maker.

## Fulfilment And Resolution

| Field | Purpose |
|---|---|
| FulfilmentStarted | Idempotency/control flag for the simulated fulfilment path |
| FulfilmentNotes | Notes from the mock fulfilment/resolution step |
| FulfilmentError | Error capture for resolution or simulated fulfilment failures |

Fulfilment/provisioning is mocked or simulated in this portfolio project.

## SLA And Escalation

| Field | Purpose |
|---|---|
| ReminderCount | Number of reminder attempts recorded |
| LastReminderOn | Timestamp of the last reminder |
| Escalated | Escalation state flag |
| EscalationCount | Number of escalation attempts recorded |
| EscalatedOn | Timestamp of escalation |

## Metrics

| Field | Purpose |
|---|---|
| TimeToApproveHours | Time between submission and decision |
| TimeToResolutionHours | Time between submission and resolution |
| PostDecisionResolutionHours | Time between decision and resolution |

## Sensitive/Internal Fields

Consider Field Security Profile controls for RequestorAadObjectId, CorrelationId, ApprovalError, FulfilmentError, FulfilmentNotes, and other operational fields where appropriate.
