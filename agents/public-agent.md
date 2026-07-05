# Access Request Agent (Public)

## Purpose

**Access Request Agent (Public)** is the low-friction portfolio/demo variant of the Access Request & Approval Agent.

It lets reviewers understand the conversational request experience without needing access to a private tenant or authenticated environment.

## Behaviour

The public agent collects:

- ApplicationName
- AccessLevel
- Justification
- DurationDays
- RequestorEmail / UPN as conversational input
- ApproverEmail

After validation, the agent calls Flow A — Create Request. The created Access Requests row becomes the authoritative record.

## Public Mode Identity Limitation

Public mode may use a requester email typed by the user. That is useful for demo accessibility, but it is not the secure enterprise pattern.

For trusted identity capture, use **Access Request Agent (Internal Auth / Secure)** or **Access Request (Secure)**, where requester identity can be captured from Entra ID context.

## Admin Access Gate

When AccessLevel is Admin, the topic should present a policy acknowledgement gate before calling Flow A — Create Request.

This demonstrates a governed intake pattern, but it should not be presented as a production-grade access control by itself.

## Flow Calls

| Flow | Use |
|---|---|
| Flow A — Create Request | Creates the Access Requests Dataverse row and returns structured confirmation |
| GetStatus | Retrieves current lifecycle state from Dataverse |

## Demo Safety

Use fictional data only. Do not publish screenshots containing real email addresses, private approver names, tenant details, environment URLs, endpoint URLs, connection details, client IDs, secrets, or live access data.
