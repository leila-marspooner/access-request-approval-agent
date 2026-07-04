# Flow D — SLA Reminder / Escalation Watcher

## Purpose

Flow D — SLA Reminder / Escalation Watcher monitors pending approvals and prevents requests from silently stalling.

## Trigger

Scheduled recurrence, usually daily or aligned to demo needs.

## Configuration

- `lai_SLA_ReminderHours`
- `lai_SLA_MaxReminders`
- `lai_EscalationRecipient`
- `lai_ApprovalChannelOrMailbox`

## Main Actions

- List Access Requests records.
- Find Pending records where approval was sent and no decision has been returned.
- Compare timestamps against the configured SLA threshold.
- Send reminder to the approver.
- Increment ReminderCount.
- Stamp LastReminderOn.
- Escalate after configured reminder limit.
- Increment EscalationCount.
- Stamp EscalatedOn.
- Set EscalatedFlag where used.

## Governance Notes

This flow demonstrates autonomous operations and lifecycle monitoring. It should process records safely so one failed item does not break the whole sweep.
