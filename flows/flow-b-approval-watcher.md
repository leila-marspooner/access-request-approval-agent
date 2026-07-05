# Flow B — Approval Watcher

## Purpose

Flow B — Approval Watcher monitors Dataverse for pending access requests and sends a Teams Adaptive Card to the approver.

It captures the human decision and writes the result back to the Access Requests row.

## Trigger

Dataverse row added or modified.

## Detection Logic

The watcher should only process rows that meet the approval criteria, such as:

- Status is Pending
- ApprovalRequestSent is not true
- ApproverEmail is present

## Key Dataverse Fields Read

- Status
- Status Reason
- ApproverEmail
- ApprovalRequestSent
- ApprovalRequestSentOn
- ApplicationName
- AccessLevel
- Justification
- DurationDays
- CaseSummary
- CorrelationId

## Key Dataverse Fields Updated

- ApprovalRequestSent
- ApprovalRequestSentOn
- ApproverDecision
- ApproverComments
- DecisionOn
- Status
- Status Reason
- ApprovalError where required

## Idempotency

ApprovalRequestSent and ApprovalRequestSentOn help prevent duplicate Teams Adaptive Cards.

Recommended sequence:

1. Confirm the request is pending.
2. Check ApprovalRequestSent.
3. Stamp ApprovalRequestSent and ApprovalRequestSentOn before sending the card.
4. Send the Teams Adaptive Card.
5. Exit cleanly if the watcher retriggers after the card has already been sent.

## Error Handling

Approval posting, response capture, and Dataverse update failures should be written to ApprovalError where practical.

Errors shown to users or approvers should be concise and should not reveal private run data, connection details, or tenant-specific configuration.

## Screenshot Evidence

Available evidence:

- [Flow B — Approval Watcher overview](../screenshots/approvalWatcherOverview.png)
- [Teams Adaptive Card approval](../screenshots/teamsAccessRequestApproved.jpeg)

Still to capture (redacted):

- Dataverse trigger
- ApprovalRequestSent idempotency check
- Decision write-back
- ApprovalError path where available
