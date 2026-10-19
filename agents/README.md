# Agent Design

This folder documents the Copilot Studio agent layer for the **Access Request & Approval Agent**.

The agent is the conversational entry point, not the system of record. It guides requesters through structured intake, validates that the minimum request details are present, explains the approval policy where needed, and calls Power Automate flows. Dataverse and watcher flows own the governed lifecycle after submission.

## Agent Variants

| Variant | Purpose | Identity Pattern |
|---|---|---|
| Access Request Agent (Public) | Public/demo mode for portfolio review and low-friction walkthroughs | Requester and approver emails are conversational demo inputs |
| Access Request Agent (Internal Auth / Secure) | Authenticated intake and manager lookup | Signed-in email and principal name; missing required claims stop intake |
| Access Request (Secure) | Secure variant label used in build evidence | Same secure identity pattern as the internal authenticated agent |

The public variant makes the project easy to review. The secure variant demonstrates the stronger enterprise pattern where requester identity is captured from sign-in rather than trusted from typed input.

## Core Topics

| Topic | Purpose | Flow Called |
|---|---|---|
| Request Access | Collects request details, applies basic validation, and submits the request | Flow A — Create Request |
| GetStatus / Check Request Status | Looks up the current request state from Dataverse | GetStatus |

## Request Access Topic Design

Both variants collect ApplicationName, AccessLevel, Justification and DurationDays and call Flow A — Create Request after validation and confirmation.

- **Public/demo:** requester email and ApproverEmail are conversational inputs. Typed identity is demo data, not proof of ownership.
- **Secure:** the topic checks sign-in, principal name and email before intake, sources requester email from signed-in context, and passes principal name through the existing `ApproverEmail` flow input. Flow A contains manager lookup and a manager-email availability branch; the requester is not asked to select an approver.

See [secure intake and input mappings](secure-agent.md#request-intake-and-manager-lookup) for the exact visible contract. `RequestorAadObjectId` belongs to the wider schema/design but its capture is not shown in the six-input call. The screenshots do not establish that the current secure flow call is interchangeable with the public/demo input contract.

## Admin Access Policy Gate

Admin-level access is higher risk, so the agent includes a visible policy acknowledgement gate before the request is submitted.

The gate is not a security boundary by itself. It is a portfolio demonstration of how conversational intake can enforce an explicit acknowledgement before automation begins. Real production use would require additional policy, identity, approver validation, and governance controls.

## GetStatus Topic Design

The GetStatus topic is deliberately Dataverse-backed. It should not rely on chat memory or Teams messages as the source of truth.

The captured conversations show request details and summary on an allowed lookup, and a generic permission/not-found message on denied lookups. The [GetStatus page](../flows/flow-get-status.md) separates these observations from ownership enforcement still to verify.

The documented response contract can include:

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

## Screenshot Evidence

Available evidence:

- [Copilot Studio agent overview](../screenshots/copilotStudioOverview.png)
- [Secure Request Access topic — identity gates and intake](../screenshots/copilotTopicRequestAccess1.png)
- [Earlier request submission topic configuration](../screenshots/copilotTopicRequestAccessTestSuccess.png)
- [Earlier validation topic configuration](../screenshots/copilotTopicRequestAccessTestFail.png)
- [Check Access Status topic — full flow](../screenshots/copilotTopicCheckAccessStatusFullFlow.png)
- [Secure agent authentication settings](../screenshots/copilotStudioAuthenticationSettings.png)

The secure topic image already includes the Admin acknowledgement gate and Flow A call. Identity-gate runtime tests and expanded mappings remain outstanding; see the [secure evidence limits](secure-agent.md#evidence-limits-and-outstanding-tests).

## Public Safety Notes

Do not publish exported agent configuration or screenshots showing tenant URLs, environment URLs, app registration IDs, client IDs, client secrets, endpoint URLs, real email addresses, private approver names, or personal maker account details.

Use fictional demo request data and redacted screenshots only.

## Future Improvements

- Add richer requester-facing status messages.
- Add cancellation or withdrawal topic.
- Add duplicate-request detection before submission.
- Add approver validation against Entra ID.
- Add clearer help text for secure versus public/demo mode.
