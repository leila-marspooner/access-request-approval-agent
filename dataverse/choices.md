# Choices

## Status

Recommended Status values:

- Draft
- Pending
- Approved
- Rejected
- Fulfilled

Status is the authoritative lifecycle state and should drive watcher-flow behaviour.

## AccessLevel

Recommended AccessLevel values:

- Standard
- Elevated
- Admin

Admin access should trigger a visible policy acknowledgement gate before Flow A — Create Request is called.

## ApproverDecision

Recommended ApproverDecision values:

- Approved
- Rejected

ApproverDecision should be written by Flow B — Approval Watcher after the Teams Adaptive Card response.
