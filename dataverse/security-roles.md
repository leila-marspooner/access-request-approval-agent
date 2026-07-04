# Security Roles

This project demonstrates RBAC and Least privilege thinking for a governed access request process.

## Recommended Roles

| Role | Intended Access |
|---|---|
| Access Request - Requester | Create requests and read own records |
| Access Request - Approver | Read assigned requests and update approval decision fields |
| Access Request - Fulfilment/Ops | Read/write fulfilment and operational fields |
| Access Request - Admin | Manage records, configuration, views, and operational oversight |
| Access Request - Automation Service | Optional service role for Power Automate execution |

## Guardrails

- Requesters should not delete, assign, or share records unless explicitly required.
- Approvers should not edit original request details after submission.
- Fulfilment/Ops users should not receive unnecessary administrative privileges.
- Admin access should be limited and reviewed.
- Draft-only editing should prevent key request fields from changing after Status leaves Draft.

## Evidence Note

Only describe security roles as fully implemented where public-safe screenshot or solution evidence exists. Otherwise, describe them as documented governance design.
