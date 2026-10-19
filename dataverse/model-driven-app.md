# Model-driven App

The model-driven app provides operational visibility over the Access Requests lifecycle.

It is designed for approvers, fulfilment/operations users, and admins who need to review state, decisions, errors, SLA activity, and audit evidence without opening every Power Automate run.

## Purpose

The app demonstrates:

- Dataverse as the system of record
- Request lifecycle visibility
- Approval decision review
- Simulated fulfilment and resolution tracking
- SLA reminder and escalation monitoring
- Error capture and troubleshooting
- Audit history evidence

## Main Views

Recommended views include:

- Active Access Requests
- My Requests
- Pending Approval
- Approved
- Rejected
- Overdue / Reminder Sent
- Escalated
- Records With Errors
- Service Performance / Metrics

## Main Form Sections

| Section | Example Fields |
|---|---|
| Request details | ApplicationName, AccessLevel, Justification, DurationDays |
| Requester identity | RequestorEmail, RequestorAadObjectId |
| Approval | ApprovalRequestSent, ApprovalRequestSentOn, ApproverDecision, ApproverComments, DecisionOn, ApprovalError |
| Summary and traceability | CaseSummary, CorrelationId |
| Resolution / mock fulfilment | ResolvedOn, FulfilmentStarted, FulfilmentNotes, FulfilmentError |
| SLA / operations | ReminderCount, LastReminderOn, Escalated, EscalationCount, EscalatedOn |
| Metrics | TimeToApproveHours, TimeToResolutionHours, PostDecisionResolutionHours |
| Audit | Dataverse audit history, Created By, Created On, Modified By, Modified On, Owner |

## Recruiter Value

The app shows that the project is not only a chatbot. It demonstrates operational design: stateful records, supportable error fields, role-aware views, auditability, and lifecycle monitoring.

## Screenshot Evidence

Available redacted screenshots:

- [Main request form](../screenshots/modelDrivenForm.png)
- [Summary and traceability tab](../screenshots/modelAppRecordSummaryTraceabilityTab.png)
- [SLA / operations tab](../screenshots/modelDrivenAppFormSLA.png)
- [Escalated requests view](../screenshots/modelDrivenAppEscalatedTickets.png)
- [Service performance view](../screenshots/modelAppDashboardServicePerformance.png)
- [Audit history](../screenshots/auditHistory.png)

These show operational views, not verification of the current SLA sweep's counter expressions or failure continuation.

Do not expose live Dataverse data, real user names, real emails, environment URLs, tenant values, or private audit details.
