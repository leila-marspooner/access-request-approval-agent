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

This is the documented public/demo intake contract. After validation, the agent calls Flow A — Create Request; the created Access Requests row becomes the authoritative record. The current secure screenshots show a different identity/routing mapping and do not verify this public contract against the latest flow configuration.

## Public Mode Identity Limitation

Public mode may use a requester email typed by the user. That is useful for demo accessibility, but it is not the secure enterprise pattern.

The [secure variant](secure-agent.md) checks signed-in identity and uses a manager lookup path. Public typed email is not an authorization boundary; the secure status examples do not establish safe public access to request records.

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
