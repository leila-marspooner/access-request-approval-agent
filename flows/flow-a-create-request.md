# Flow A — Create Request

## Purpose

Flow A — Create Request is the Copilot-triggered flow that validates the request, generates a CorrelationId, calls Flow H — SummariseCase, creates the Dataverse Access Requests row, and returns structured details to Copilot Studio.

## Inputs

- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail / UPN
- RequestorAadObjectId where available
- ApproverEmail

## Main Actions

- Validate required inputs.
- Generate CorrelationId.
- Call Flow H — SummariseCase.
- Create the Access Requests Dataverse row.
- Set Status to Pending.
- Store CaseSummary.
- Return request reference, Status, CaseSummary, and CorrelationId.

## Governance Notes

- Uses TRY/CATCH and Error capture patterns.
- Treats Dataverse as system of record.
- Allows request creation to continue with a fallback CaseSummary if AI summarisation fails.
- Does not send approval directly; Flow B — Approval Watcher handles approval routing.
