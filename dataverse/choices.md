# Choices

This file documents the intended choice/value design for the Access Requests table.

Exact Dataverse option values should be verified against the solution before publishing generated metadata.

## Status

Status is the authoritative lifecycle state and should drive watcher-flow behaviour.

Typical values may include:

- Draft
- Pending
- Approved
- Rejected
- Fulfilled or Resolved, depending on the final Dataverse configuration

The captured GetStatus response displays `Active`. Its source expression is not shown, so it must not be treated as proof of approval, real provisioning or a new lifecycle choice. Verify Status/Status Reason mappings against solution metadata before publishing exact option values.

## Status Reason

Status Reason can be used to make lifecycle state more explicit, for example:

- Awaiting Approval
- Approved - Pending Resolution
- Rejected - Closed
- Resolved
- Error
- Escalated

## AccessLevel

Recommended values:

- Standard
- Elevated
- Admin

Admin access should trigger a visible policy acknowledgement gate before Flow A — Create Request is called.

## ApproverDecision

Recommended values:

- Approved
- Rejected

ApproverDecision is written by Flow B — Approval Watcher after the Teams Adaptive Card response. The AI-generated CaseSummary must not populate this field.
