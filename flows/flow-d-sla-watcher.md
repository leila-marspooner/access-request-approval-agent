# Flow D — SLA Reminder / Escalation Watcher

## Purpose

Flow D — SLA Reminder / Escalation Watcher monitors pending approvals so requests do not silently stall.

It demonstrates autonomous operational monitoring after the requester has left the chat experience.

## Trigger

Scheduled recurrence, usually daily or aligned to demo needs.

## Configuration

Use environment variables or other safe configuration patterns for:

- SLA reminder threshold
- Maximum reminders
- Escalation recipient
- Approval routing destination

Do not hardcode or publish real recipients, channel IDs, tenant values, or endpoint URLs.

## Key Dataverse Fields Read

- Status
- Status Reason
- ApprovalRequestSent
- ApprovalRequestSentOn
- DecisionOn
- ApproverEmail
- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn

## Key Dataverse Fields Updated

- ReminderCount
- LastReminderOn
- Escalated
- EscalationCount
- EscalatedOn

## Reminder Pattern

1. Find pending requests where approval was sent and no decision has been returned.
2. Compare ApprovalRequestSentOn and LastReminderOn against the configured SLA threshold.
3. Send a reminder where appropriate.
4. Increment ReminderCount.
5. Stamp LastReminderOn.

## Escalation Pattern

1. Check whether ReminderCount has reached the configured limit.
2. Set Escalated.
3. Increment EscalationCount.
4. Stamp EscalatedOn.
5. Avoid repeated uncontrolled escalation of the same request.

## Error Handling

The watcher should handle item-level failures where possible so one problematic record does not break the entire scheduled sweep.

Where implemented, item errors should be captured on the request row or in an operational log.

## Screenshot Evidence

Add redacted screenshots for:

- Recurrence trigger
- Query/filter for overdue pending approvals
- ReminderCount and LastReminderOn update
- Escalated, EscalationCount, and EscalatedOn update
- Item-level error handling where available
