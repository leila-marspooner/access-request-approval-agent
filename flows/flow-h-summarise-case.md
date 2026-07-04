# Flow H — SummariseCase

## Purpose

Flow H — SummariseCase is a reusable BYOM child flow that generates a concise CaseSummary for the approver.

## Inputs

- ApplicationName
- AccessLevel
- DurationDays
- Justification
- RequestorEmail / UPN

## Output

- CaseSummary

## Configuration

The BYOM pattern should use Environment variables and Key Vault-backed secret handling where applicable:

- `lai_BYOM_Endpoint`
- `lai_BYOM_DeploymentName`
- `lai_BYOM_ApiVersion`
- `lai_BYOM_ApiKey`
- `lai_BYOM_SystemPrompt`

## Governance Notes

- Azure OpenAI supports the reviewer but does not make the approval decision.
- Model configuration should not be hardcoded.
- Secrets, keys, endpoints, and tenant-specific values must not be published.
- If the model call fails or throttles, return a deterministic fallback summary so Flow A — Create Request can continue.
