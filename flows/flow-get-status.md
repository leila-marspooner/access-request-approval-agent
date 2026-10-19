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

The flow looks up request information in Dataverse. The intended restriction is that a requester can see only their own permitted requests. Captured conversations demonstrate an allowed response and generic denials; they do not expose the flow-side filter or authorization expression.

The documented RequestorEmail / UPN input above is not proof of trusted identity binding. A typed email alone must not be treated as authorization.

## Documented Response Fields

The wider response contract can include the fields below. The captured allowed response specifically shows request reference, application, displayed status, submitted timestamp and summary. It does not demonstrate every optional field.

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
- Return details only after an ownership/permission check using trusted identity; the exact implementation still needs inspection.
- Avoid exposing internal-only errors or record existence in denial messages.

## Screenshot Evidence

Available evidence:

- [GetStatus flow](../screenshots/getStatus.png)
- [Check Access Status topic — full flow](../screenshots/copilotTopicCheckAccessStatusFullFlow.png)

## Observed Status Responses

![Status lookup returns request details and summary](../screenshots/copilotStatusAllowed.png)

AR001025 returns application, displayed Status `Active`, submitted timestamp and summary. `Active` is the displayed status in this response, not proof that access was provisioned or a new approval lifecycle state.

![Same request reference receives a permission-safe denial](../screenshots/copilotStatusOwnershipDenied.png)

Another captured conversation using AR001025 receives “I couldn't find a request you have permission to view.” This supports an ownership-aware user experience, but the screenshots do not show which identities were used or the underlying ownership expression.

![AR000000 lookup receives a generic denial and retry prompt](../screenshots/copilotStatusNotFound.png)

AR000000 receives the same generic response and an option to check another number. The response does not reveal whether a request exists. The screenshot alone does not independently verify that the reference is absent from Dataverse.

## Ownership Restrictions and Verification

The permitted response and generic denials demonstrate the user-visible restriction. The older flow/topic canvases remain structural evidence, but do not establish the current ownership predicate, caller identity source or execution identity.

Outstanding verification: inspect the expanded lookup/filter and authorization branch; run controlled same-user, cross-user, missing-identity and nonexistent-reference cases; confirm denied cases return no record details. Verify the public/demo path separately because typed requester email is not trusted identity.
