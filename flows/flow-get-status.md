# GetStatus

## Purpose

GetStatus is a Copilot-triggered helper flow that returns the current lifecycle state of an access request from Dataverse.

It gives requesters a structured status-check path without relying on chat memory or Teams messages.

## Trigger

Called from Copilot Studio by the GetStatus or Check Request Status topic.

## Inputs

- Request Number or request identifier
- RequestorEmail / UPN where needed for lookup or filtering

## Dataverse Lookup

The flow retrieves the matching Access Requests row and returns requester-safe lifecycle information.

## Outputs

- Status
- Status Reason
- SubmittedOn
- DecisionOn where available
- ResolvedOn where available
- ApproverDecision where available
- CaseSummary where appropriate
- Request not found message where appropriate
- Next-step message for the requester

## Governance Notes

- Dataverse remains the system of record.
- In a production design, ownership or identity checks would be required before returning request details.
- Responses should avoid exposing internal-only error fields or private operational details.

## Screenshot Evidence

Add redacted screenshots for:

- Copilot Studio GetStatus topic
- Flow lookup action
- Successful status response
- Request-not-found path
