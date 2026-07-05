# Agent Design

This folder documents the Copilot Studio agent layer for the **Access Request & Approval Agent**.

The agent is the conversational entry point, not the system of record. It guides requesters through structured intake, validates that the minimum request details are present, explains the approval policy where needed, and calls Power Automate flows. Dataverse and watcher flows own the governed lifecycle after submission.

## Agent Variants

| Variant | Purpose | Identity Pattern |
|---|---|---|
| Access Request Agent (Public) | Public/demo mode for portfolio review and low-friction walkthroughs | RequestorEmail / UPN may be collected conversationally |
| Access Request Agent (Internal Auth / Secure) | Enterprise-aligned variant for authenticated users | RequestorEmail / UPN and RequestorAadObjectId are captured from Entra ID context where available |
| Access Request (Secure) | Secure variant label used in build evidence | Same secure identity pattern as the internal authenticated agent |

The public variant makes the project easy to review. The secure variant demonstrates the stronger enterprise pattern where requester identity is captured from sign-in rather than trusted from typed input.

## Core Topics

| Topic | Purpose | Flow Called |
|---|---|---|
| Request Access | Collects request details, applies basic validation, and submits the request | Flow A — Create Request |
| GetStatus / Check Request Status | Looks up the current request state from Dataverse | GetStatus |

## Request Access Topic Design

The Request Access topic collects the core information needed to create an Access Requests row:

- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail / UPN
- RequestorAadObjectId in secure mode where available
- ApproverEmail

The agent should keep the interaction clear and practical. It should ask follow-up questions when required details are missing or too vague, then pass structured values to Flow A — Create Request.

## Admin Access Policy Gate

Admin-level access is higher risk, so the agent includes a visible policy acknowledgement gate before the request is submitted.

The gate is not a security boundary by itself. It is a portfolio demonstration of how conversational intake can enforce an explicit acknowledgement before automation begins. Real production use would require additional policy, identity, approver validation, and governance controls.

## GetStatus Topic Design

The GetStatus topic is deliberately Dataverse-backed. It should not rely on chat memory or Teams messages as the source of truth.

Typical response fields include:

- Request Number or request reference
- Status
- Status Reason
- SubmittedOn
- DecisionOn where available
- ResolvedOn where available
- ApproverDecision where available
- A concise next-step message for the requester

## Why Lifecycle Logic Stays In Power Automate

Approval, resolution, SLA monitoring, and fulfilment logic are kept in Power Automate rather than the chat layer because those steps need:

- Deterministic branching
- Retry control
- Run history
- Idempotency checks
- TRY/CATCH-style error capture
- Dataverse write-back
- Supportability outside the conversation

Copilot Studio handles the interaction. Power Automate and Dataverse control the business process.

## What The Agent Deliberately Does Not Do

The agent does not:

- Approve or reject access.
- Provision real access.
- Replace Microsoft Entra ID Governance, ITSM, or a production access management platform.
- Treat AI output as an approval decision.
- Store the authoritative lifecycle state in chat history.

Fulfilment/provisioning is mocked or simulated in this portfolio project unless future evidence shows a real provisioning integration.

## Screenshot Evidence To Include

Add public-safe screenshots for:

- Request Access topic overview
- Admin access policy acknowledgement gate
- Public agent request intake
- Secure agent authentication or identity capture evidence after redaction
- Flow A — Create Request call from the topic
- GetStatus topic calling the GetStatus flow
- Successful request submission response
- Failed or validation response where useful

## Public Safety Notes

Do not publish exported agent configuration or screenshots showing tenant URLs, environment URLs, app registration IDs, client IDs, client secrets, endpoint URLs, real email addresses, private approver names, or personal maker account details.

Use fictional demo request data and redacted screenshots only.

## Future Improvements

- Add richer requester-facing status messages.
- Add cancellation or withdrawal topic.
- Add duplicate-request detection before submission.
- Add approver validation against Entra ID.
- Add clearer help text for secure versus public/demo mode.
