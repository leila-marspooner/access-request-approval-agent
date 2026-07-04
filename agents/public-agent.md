# Access Request Agent (Public)

## Purpose

Access Request Agent (Public) is the low-friction demo variant of the Access Request & Approval Agent. It lets a portfolio reviewer see the conversational request experience without requiring tenant sign-in.

## Behaviour

The public agent collects:

- Application name
- Access level
- Business justification
- Duration in days
- RequestorEmail / UPN as a conversational input
- ApproverEmail

If Admin access is requested, the agent shows a policy acknowledgement gate before Flow A — Create Request is called.

## Important Limitation

Because this is public/demo mode, requester identity may be typed by the user. That is useful for demo accessibility but is not the secure enterprise pattern. The secure variant should be used when trusted identity capture is required.

## Flow Calls

- Flow A — Create Request creates the Dataverse Access Requests row.
- GetStatus retrieves request status from Dataverse.

## Demo Notes

Use redacted demo values only. Do not use real email addresses, private approver names, tenant-specific application names, or live access data in public screenshots.
