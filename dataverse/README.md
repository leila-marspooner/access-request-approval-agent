# Dataverse

This folder documents the Dataverse design for the Access Request & Approval Agent.

## Main Table

- Access Requests

The Access Requests table is the system of record for the request lifecycle. It stores request input, identity fields, Status, CaseSummary, CorrelationId, approval state, fulfilment state, SLA fields, escalation fields, and Error capture.

## Dataverse Design Principle

Dataverse is used as the lifecycle authority, not just passive storage. Power Automate watcher flows read from and write back to Access Requests so the Model-driven app can show operational state without requiring users to inspect every flow run.

## Related Files

- [Access Requests table](access-requests-table.md)
- [Columns](columns.md)
- [Choices](choices.md)
- [Security roles](security-roles.md)
- [Model-driven app](model-driven-app.md)
