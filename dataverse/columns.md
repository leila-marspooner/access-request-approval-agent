# Columns

The following columns are documented for the Access Requests table. Logical names should be verified against the final Dataverse solution before publishing screenshots or generated schema exports.

| Display Name | Purpose |
|---|---|
| ApplicationName | Requested application or business system |
| AccessLevel | Requested access level |
| Justification | Business reason for access |
| DurationDays | Requested duration |
| RequestorEmail / UPN | Requester email or signed-in UPN |
| RequestorAadObjectId | Entra ID object ID in secure mode |
| ApproverEmail | Approver address used by approval routing |
| Status | Authoritative lifecycle state |
| SubmittedOn | Request submission timestamp |
| CaseSummary | BYOM summary for reviewer support |
| CorrelationId | End-to-end trace identifier |
| ApprovalRequestSent | Idempotency flag for Teams Adaptive Card |
| ApprovalRequestSentOn | Timestamp for approval card send |
| ApproverDecision | Approved or Rejected |
| ApproverComments | Human approval comments |
| DecisionOn | Decision timestamp |
| FulfillmentStarted | Optional fulfilment Idempotency flag |
| FulfilledOn | Simulated fulfilment completion timestamp |
| FulfillmentNotes | Mock fulfilment notes |
| ApprovalError | Approval watcher error capture |
| FulfillmentError | Resolution/fulfilment error capture |
| ReminderCount | SLA reminder counter |
| LastReminderOn | Last reminder timestamp |
| EscalationCount | Escalation counter |
| EscalatedOn | Escalation timestamp |
| EscalatedFlag | Escalation status flag |

## Sensitive/Internal Fields

Consider Field Security Profile controls for fields such as RequestorAadObjectId, CorrelationId, ApprovalError, FulfillmentError, FulfillmentNotes, and any internal operational fields.
