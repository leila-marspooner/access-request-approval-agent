# Flow D — SLA Reminder / Escalation Watcher

## Purpose and Trigger

Flow D — SLA Reminder / Escalation Watcher is a scheduled Power Automate flow for overdue pending approvals. The canvas shows Recurrence, TRY, Fetch Approval Requests, Apply to each and a per-request Still Pending condition. The recurrence interval and query/filter expressions are not expanded.

## Configuration and Dataverse State

The documented configuration uses environment variables for reminder threshold, maximum reminders, escalation recipient and routing destination. Do not publish their private values.

The lifecycle design uses Status, Status Reason, ApprovalRequestSent, ApprovalRequestSentOn, DecisionOn and ApproverEmail to identify pending approvals, with ReminderCount, LastReminderOn, Escalated, EscalationCount and EscalatedOn for monitoring state. The exact query, condition and row-update mappings remain to verify.

## Reminder and Escalation Logic

Inside Still Pending, the expanded canvas shows ThresholdHours, DueTime, Overdue, NewReminderCount, MaxReminders and Escalate. The escalation branch contains a Teams escalation action and DV — Stamp Escalation; the other branch contains a Teams reminder and DV — Update Reminder Counters.

The documented write-back design is:

| Branch | Intended fields |
|---|---|
| Reminder | Increment ReminderCount and stamp LastReminderOn |
| Escalation | Set Escalated, increment EscalationCount and stamp EscalatedOn |

These action names establish the branch structure. Their collapsed expressions do not establish the exact thresholds, timestamp calculations, duplicate-suppression checks or values written. The displayed order is Teams notification followed by Dataverse update; notification delivery and write-back must be checked in runtime evidence.

## Error Handling

Within Apply to each, Process current request is followed by Handle request error. That handler contains Filter failed request steps, Compose run link and a Teams message action. An outer CATCH contains failed-step filtering and failure-detail composition.

This is configured error handling. The run-after settings, filter expressions, message destination and lower part of the outer CATCH are not shown. The screenshots do not prove that an item failure is reported successfully, that later items continue, or that all failures are persisted to Dataverse. Isolating a bad record so the sweep can continue is the intended resilience goal, pending verification.

The earlier failed-TRY/successful-CATCH run is separate evidence of error-path execution in an earlier capture. It is not a test of this per-request handler or of continuation after item failure.

## Screenshot Evidence

![SLA watcher recurrence and nested error scopes](../screenshots/slaWatcherOverview.png)

Scheduled sweep, per-request error handler and visible portion of outer CATCH; configuration view.

![SLA overdue decision with reminder and escalation branches](../screenshots/slaMaxRemindersGate.png)

Overdue and reminder-limit branching with Teams and Dataverse actions; configuration view.

- [Earlier failed TRY with successful CATCH run](../screenshots/slaTryFailCatchSuccess.png)
- [Teams approval reminder example](../screenshots/teamsApprovalReminder.png)

## Evidence Limits and Outstanding Tests

- Expand recurrence, overdue query, Still Pending condition and threshold expressions.
- Verify reminder/escalation counter, timestamp and duplicate-suppression mappings.
- Inspect run-after settings for per-request handling, loop continuation and outer CATCH.
- Run a multi-record sweep with a forced item failure and show later records processing.
- Verify reminder, escalation and error-message delivery, Dataverse write-back and outer-CATCH handling separately.
