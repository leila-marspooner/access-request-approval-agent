# Project Summary

## Purpose

The Access Request & Approval Agent is a Power Platform / Copilot Studio portfolio case study that demonstrates a governed access request and approval lifecycle.

It shows how Copilot Studio can collect structured request details, Power Automate can orchestrate approval and lifecycle processing, Dataverse can act as the system of record, Teams Adaptive Cards can capture Human-in-the-loop approval, and Azure OpenAI BYOM summarisation can support reviewers without making the decision.

This is not a production access management system and does not replace Microsoft Entra ID Governance, a production identity governance platform, or an enterprise ITSM tool.

## Business Problem

Access requests are often submitted through informal email, chat, or spreadsheet processes. That can lead to incomplete requests, lost approvals, weak audit trails, duplicate follow-up, unclear ownership, and poor visibility into overdue items.

This project demonstrates a more governed pattern:

- Structured conversational intake
- Dataverse as system of record
- Deterministic Power Automate lifecycle flows
- Teams Adaptive Card approval
- Status tracking through GetStatus
- Model-driven app visibility
- RBAC, Dataverse audit history, Field Security Profile, and Error capture

## Main User Journeys

### Submit Access Request

The requester uses the Access Request Agent to provide the application, access level, business justification, duration, and approver. Admin access requests include a policy acknowledgement gate before submission.

### Approve Or Reject Request

Flow B — Approval Watcher sends a Teams Adaptive Card to the approver. The approver reviews request details and CaseSummary, then submits ApproverDecision and ApproverComments.

### Resolve Request

Flow C — Resolution Watcher processes approved and rejected outcomes. Approved requests use mocked/simulated fulfilment unless real provisioning evidence is added later.

### Check Request Status

The GetStatus flow retrieves lifecycle status from Dataverse and returns the latest Status and key timestamps to Copilot Studio.

### Monitor SLA

Flow D — SLA Reminder / Escalation Watcher checks pending approvals, sends reminders, increments ReminderCount, stamps LastReminderOn, and escalates after configured thresholds.

## Core Components

| Component | Role |
|---|---|
| Access Request Agent (Public) | Public/demo conversational intake with manually supplied requester identity |
| Access Request Agent (Internal Auth / Secure) | Secure mode using Entra ID identity capture |
| Access Request (Secure) | Secure variant name used for authenticated demonstration |
| Flow A — Create Request | Creates the Dataverse Access Requests row and returns request details |
| Flow H — SummariseCase | Generates CaseSummary through Azure OpenAI BYOM with fallback handling |
| GetStatus | Returns current request status from Dataverse |
| Flow B — Approval Watcher | Sends Teams Adaptive Card approval and captures the decision |
| Flow C — Resolution Watcher | Processes approved/rejected outcomes and simulated fulfilment |
| Flow D — SLA Reminder / Escalation Watcher | Monitors overdue approvals and records reminders/escalations |
| Model-driven app | Operational view of requests, errors, audit fields, and SLA state |

## Dataverse System Of Record

The Access Requests table holds the authoritative lifecycle state. Important fields include Status, CaseSummary, CorrelationId, RequestorEmail / UPN, RequestorAadObjectId, ApproverEmail, ApprovalRequestSent, ApprovalRequestSentOn, ApproverDecision, ApproverComments, DecisionOn, FulfilledOn, FulfillmentStarted, FulfillmentNotes, ApprovalError, FulfillmentError, ReminderCount, LastReminderOn, EscalationCount, EscalatedOn, and EscalatedFlag.

## Governance Themes

- RBAC and Least privilege
- Draft-only editing
- Field Security Profile for sensitive/internal fields
- Dataverse audit history
- Idempotency for approvals, fulfilment, and reminders
- TRY/CATCH and Error capture
- CorrelationId for traceability
- Environment variables and Connection references
- Managed solution and Unmanaged solution ALM discipline
- Release checklist and Definition of Done

## Current Portfolio Positioning

Position this project as an enterprise-aligned portfolio/demo case study. It demonstrates strong architecture, governance thinking, and Power Platform implementation patterns, while clearly stating that production deployment would require further hardening, security review, operational monitoring, DLP validation, and real provisioning integration.
