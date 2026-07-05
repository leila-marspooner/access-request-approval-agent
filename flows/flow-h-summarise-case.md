# Flow H — SummariseCase

## Purpose

Flow H — SummariseCase is a reusable child flow that generates CaseSummary for the approver.

The summary helps the human reviewer understand the request quickly. It does not approve, reject, risk-score, or provision access.

## Trigger

Called by Flow A — Create Request.

## Inputs

- ApplicationName
- AccessLevel
- DurationDays
- Justification
- RequestorEmail / UPN

## Output

- CaseSummary

## BYOM Azure OpenAI Pattern

The flow uses a Bring Your Own Model pattern with Azure OpenAI. Configuration should be externalised through environment variables and secure secret handling where implemented.

Recommended configuration areas:

- BYOM endpoint
- BYOM deployment name
- BYOM API version
- BYOM system prompt
- BYOM API key or Key Vault-backed secret reference

Do not publish endpoint URLs, API keys, bearer tokens, client secrets, or Key Vault secret values.

## Fallback Handling

If the model call fails, times out, or is throttled, the flow should return a deterministic fallback summary so Flow A — Create Request can continue.

The fallback should be clear and modest, for example that the request details are available for reviewer assessment but AI summarisation was unavailable.

## AI Governance

- AI output supports the approver.
- The human approver remains the decision-maker.
- The summary should not be treated as ground truth.
- The prompt should avoid unnecessary personal or sensitive data.
- CaseSummary should be reviewed in the same governance context as other operational request data.

## Screenshot Evidence

Add redacted screenshots for:

- Child flow overview
- Environment variable usage
- Key Vault pattern where implemented
- Successful CaseSummary response
- Fallback path evidence
