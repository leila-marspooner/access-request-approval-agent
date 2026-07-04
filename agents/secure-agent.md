# Access Request Agent (Internal Auth / Secure)

## Purpose

Access Request Agent (Internal Auth / Secure), also referred to as Access Request (Secure), demonstrates the enterprise pattern for authenticated access request intake.

## Identity Pattern

The secure variant uses Entra ID authentication so the requester identity can be captured from signed-in context where available.

Captured values:

- RequestorEmail / UPN
- RequestorAadObjectId

These values are written to Dataverse for auditability and traceability.

## Why This Matters

Secure identity capture avoids relying only on user-typed requester details. It creates a stronger audit trail and better aligns with enterprise access request governance.

## Behaviour

The secure agent still collects application, access level, justification, duration, and ApproverEmail. The key difference is that requester identity comes from Entra ID context rather than public user input.

## Public Documentation Safety

Do not publish app registration details, client secrets, tenant IDs, redirect URLs, environment URLs, or authentication configuration screenshots unless fully redacted.
