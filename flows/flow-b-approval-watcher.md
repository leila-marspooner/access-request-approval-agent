# Flow B — Approval Watcher

## Purpose

Flow B — Approval Watcher detects Pending requests and sends a Teams Adaptive Card to the approver.

## Trigger

Dataverse row added or modified.

## Trigger Logic

- Status = Pending
- ApprovalRequestSent is not true

## Main Actions

- Check ApprovalRequestSent.
- Set ApprovalRequestSent.
- Stamp ApprovalRequestSentOn.
- Send Teams Adaptive Card to ApproverEmail.
- Include request details and CaseSummary.
- Wait for approver response.
- Capture ApproverDecision.
- Capture ApproverComments.
- Stamp DecisionOn.
- Update Status to Approved or Rejected.

## Idempotency

ApprovalRequestSent and ApprovalRequestSentOn reduce duplicate Teams Adaptive Card risk if the Dataverse trigger fires more than once.

## Error Capture

TRY/CATCH should write approval issues to ApprovalError so the Model-driven app can show operational errors.
