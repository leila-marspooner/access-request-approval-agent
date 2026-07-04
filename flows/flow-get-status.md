# GetStatus

## Purpose

GetStatus is a Copilot-triggered helper flow that returns the current lifecycle state of an access request from Dataverse.

## Inputs

- Request ID or Dataverse row reference

## Outputs

- Status
- SubmittedOn where available
- DecisionOn where available
- FulfilledOn where available
- ApproverDecision where available
- CaseSummary where appropriate
- Next-step message for the requester

## Governance Notes

- Uses Dataverse as system of record.
- Avoids relying on chat history or Teams messages for status.
- Helps reduce support follow-up by giving requesters a structured status-check path.
