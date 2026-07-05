# Flow A — Create Request

## Purpose

Flow A — Create Request is the Copilot-triggered flow that starts the access request lifecycle.

It receives structured input from Copilot Studio, validates the request, generates CorrelationId, calls Flow H — SummariseCase, creates the Access Requests Dataverse row, and returns a structured confirmation to the agent.

## Trigger

Called from Copilot Studio by the Request Access topic.

## Inputs From Copilot Studio

- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail / UPN
- RequestorAadObjectId where available in the secure variant
- ApproverEmail

## Key Logic

1. Validate required inputs.
2. Generate CorrelationId.
3. Call Flow H — SummariseCase.
4. Use the returned CaseSummary or fallback summary.
5. Create the Dataverse Access Requests row.
6. Set the initial Status and Status Reason.
7. Return a requester-friendly response to Copilot Studio.

## Dataverse Fields Updated

- Request Number
- RequestName
- Request Title (Primary)
- RequestorEmail
- RequestorAadObjectId
- ApproverEmail
- ApplicationName
- AccessLevel
- Justification
- DurationDays
- Status
- Status Reason
- SubmittedOn
- CaseSummary
- CorrelationId

## Outputs To Copilot Studio

- Request Number or request reference
- Status
- Status Reason where useful
- CaseSummary where appropriate
- CorrelationId where safe to show
- Success or failure message

## Error Handling

Errors should be returned to the agent as safe, user-friendly messages. Technical details should be captured in flow run history or Dataverse error fields where appropriate, not exposed to the requester.

If Flow H — SummariseCase fails, Flow A should continue with a fallback CaseSummary rather than blocking request creation.

## Screenshot Evidence

Add redacted screenshots for:

- Flow trigger from Copilot Studio
- CorrelationId generation
- Flow H — SummariseCase call
- Dataverse create row action
- Successful run history with private details redacted
