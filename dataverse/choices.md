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

Use the screenshots and solution metadata as the source of truth for final published values.

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
