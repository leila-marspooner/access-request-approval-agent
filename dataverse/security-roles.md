# Security Roles

This file documents the RBAC design for the **Access Request & Approval Agent**.

The goal is least privilege: each persona should have the access needed for their role in the lifecycle and no unnecessary administrative privileges.

## Role Overview

| Role | Intended Access |
|---|---|
| Access Request - Requester | Create requests and read own records |
| Access Request - Approver | Review assigned/pending requests and update approval decision fields |
| Access Request - Fulfilment/Ops | Review and update resolution, simulated fulfilment, SLA, and error fields |
| Access Request - Admin | Manage records, configuration, views, security evidence, and operational oversight |
| Access Request - Automation Service | Optional least-privilege service role for Power Automate execution |

## Requester

Recommended privileges:

- Create Access Requests records
- Read own Access Requests records
- No delete
- No assign or share unless explicitly required
- No write access to approval, error, SLA, escalation, or internal identity fields

## Approver

Recommended privileges:

- Read relevant Access Requests records
- Update ApproverDecision, ApproverComments, DecisionOn, Status, and Status Reason where appropriate
- Read CaseSummary for reviewer support
- No access to edit original request details after submission
- No unnecessary admin privileges

## Fulfilment/Ops

Recommended privileges:

- Read request and approval context
- Update ResolvedOn, FulfilmentStarted, FulfilmentNotes, FulfilmentError, and related operational fields where appropriate
- Review ReminderCount, LastReminderOn, Escalated, EscalationCount, and EscalatedOn
- No unnecessary configuration or security-role administration

## Admin

Recommended privileges:

- Manage Access Requests records and model-driven app views/forms
- Review Dataverse audit history
- Review environment variables and connection references
- Review Field Security Profile evidence where implemented
- Support ALM and release readiness checks

## Automation Service

For production-style deployments, flows should ideally run through service-owned connections or a service account pattern rather than a personal maker connection.

Recommended privileges:

- Read/write only the fields required by the flows
- No delete unless explicitly required
- No broad system administrator dependency for routine automation

## Evidence To Include

Add redacted screenshots for:

- Requester role privileges
- Approver role privileges
- Admin role privileges
- Field Security Profile where implemented
- Model-driven app access by role where practical

Only describe roles as fully implemented where public-safe evidence exists. Otherwise, describe them as documented governance design.
