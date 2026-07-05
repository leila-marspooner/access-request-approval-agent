# Flow C — Resolution Watcher

## Purpose

Flow C — Resolution Watcher processes approved and rejected outcomes after Flow B — Approval Watcher writes the decision back to Dataverse.

For approved requests, fulfilment/provisioning is mocked or simulated in this portfolio project.

## Trigger

Dataverse row modified.

## Key Dataverse Fields Read

- Status
- Status Reason
- ApproverDecision
- ApproverComments
- DecisionOn
- RequestorEmail
- ApplicationName
- AccessLevel
- CorrelationId
- ResolvedOn
- FulfilmentStarted

## Approved Path

1. Detect an approved decision.
2. Check ResolvedOn and FulfilmentStarted to avoid duplicate processing.
3. Mark FulfilmentStarted where used.
4. Perform simulated fulfilment.
5. Stamp ResolvedOn.
6. Add FulfilmentNotes.
7. Update Status and Status Reason.
8. Notify the requester where implemented.

## Rejected Path

1. Detect a rejected decision.
2. Confirm the request has not already been resolved.
3. Stamp ResolvedOn.
4. Update Status and Status Reason.
5. Notify the requester where implemented.
6. Do not perform fulfilment.

## Dataverse Fields Updated

- Status
- Status Reason
- ResolvedOn
- FulfilmentStarted
- FulfilmentNotes
- FulfilmentError where required
- TimeToResolutionHours where implemented
- PostDecisionResolutionHours where implemented

## Idempotency

ResolvedOn, FulfilmentStarted, Status, and Status Reason help prevent duplicate resolution or repeated mock fulfilment work if the watcher retriggers.

## Error Handling

TRY/CATCH-style handling should write failures to FulfilmentError where practical.

Examples include notification failures, simulated fulfilment failures, and Dataverse update failures.

## Screenshot Evidence

Available evidence:

- [Flow C — Resolution Watcher overview](../screenshots/resolutionWatcher.png)
- [Requester notification — access approved](../screenshots/email-access-success.png)
- [Requester notification — access declined](../screenshots/email-access-declined.png)

Still to capture (redacted):

- Approved branch
- Rejected branch
- ResolvedOn and FulfilmentStarted checks
- FulfilmentNotes write-back
- FulfilmentError path where available
