# Access Request Agent (Internal Auth / Secure)

## Purpose

**Access Request Agent (Internal Auth / Secure)**, also referred to as **Access Request (Secure)**, demonstrates the authenticated enterprise-aligned variant of the Access Request & Approval Agent.

The secure variant keeps the same request lifecycle as the public agent but strengthens requester identity capture.

## Identity Pattern

The secure variant uses Entra ID authentication so requester identity can be captured from signed-in context where available.

Relevant Dataverse fields:

- RequestorEmail
- RequestorAadObjectId

This avoids relying only on user-typed requester details and creates a stronger audit trail for access request review.

## Behaviour

The secure agent still collects:

- ApplicationName
- AccessLevel
- Justification
- DurationDays
- ApproverEmail

The key difference is that requester identity should come from authenticated context rather than public user input where the channel and authentication configuration support it.

## Why This Matters

Secure identity capture supports:

- Better traceability
- Stronger audit evidence
- Reduced impersonation risk
- More enterprise-aligned request intake

This is still a portfolio/demo project. Production deployment would require additional security review, policy validation, DLP configuration, monitoring, and integration with approved provisioning systems.

## Public Documentation Safety

Do not publish app registration details, client IDs, client secrets, tenant IDs, redirect URLs containing private values, environment URLs, authentication configuration secrets, or screenshots that expose personal maker account details.
