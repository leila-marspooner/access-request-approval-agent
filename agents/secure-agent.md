# Access Request Agent (Internal Auth / Secure)

## Purpose

**Access Request Agent (Internal Auth / Secure)**, also referred to as **Access Request (Secure)**, demonstrates authenticated intake for the Access Request & Approval Agent. It uses the same six-flow lifecycle as the public/demo variant, with signed-in requester identity and a manager lookup path. Fulfilment remains simulated.

## Identity Validation

The Request Access topic checks `User.IsLoggedIn` and `User.PrincipalName` before collecting request details. If the user is not logged in or the principal name is blank, it displays a message and ends all topics. A separate blank `User.Email` check also leads to a message and termination.

On the accepted path, the topic sets `Topic.varRequestorEmail` from `System.User.Email`. The captured identity is used instead of asking the requester to type an email address. The conversation screenshot below demonstrates a refusal when the required identity could not be verified; it does not isolate which claim was missing.

`RequestorAadObjectId` is a Dataverse field in the wider secure design. Its capture is not shown in the pictured six-input topic call and must not be assumed from sign-in alone.

## Request Intake and Manager Lookup

The requester supplies ApplicationName, AccessLevel, Justification and DurationDays. The topic includes an Admin acknowledgement gate, justification validation, a 1–90 day duration check, and a submission confirmation. Declining the Admin gate or confirmation ends the topic without the flow call on that path.

The action mapping is:

| Flow input shown | Topic value |
|---|---|
| ApplicationName | Captured application |
| AccessLevel | Access level converted to text |
| Justification | Captured justification |
| RequesterEmail | `Topic.varRequestorEmail`, sourced from `System.User.Email` |
| ApproverEmail | `User.PrincipalName` |
| DurationDays | Captured duration |

`ApproverEmail` is the existing flow input name; in this secure call it carries the signed-in principal name, not a manually selected manager address. [Flow A — Create Request](../flows/flow-a-create-request.md#manager-lookup-and-missing-manager-handling) includes `Get manager (V2)` and a manager-email availability branch before row creation. The lookup parameters and Dataverse mappings are collapsed, so their exact expressions remain unverified. The flow input name must also be distinguished from the stored `ApproverEmail` field used for approval routing.

## Status Checks

GetStatus returns Dataverse-backed request information or a generic denial. [Observed responses and authorization limits](../flows/flow-get-status.md#ownership-restrictions-and-verification) describe the evidence. The screenshots do not establish the server-side ownership predicate or which identities were used in the allowed and denied sessions.

## Screenshot Evidence

![Secure request topic with identity gates and Power Automate input mappings](../screenshots/copilotTopicRequestAccess1.png)

Configuration evidence for identity checks, intake validation, acknowledgement, confirmation and the flow call. This is not an end-to-end test result.

![Request conversation refuses submission when identity cannot be verified](../screenshots/copilotRequestIdentityDenied.png)

Observed identity refusal; the sign-in code is redacted.

## Evidence Limits and Outstanding Tests

- Exercise each identity gate with controlled missing claims and verify the flow is not called.
- Verify the manager lookup parameters and stored requester/approver mappings in Flow A.
- Capture `RequestorAadObjectId` mapping separately if it is used.
- Verify status authorization with controlled same-user and cross-user tests.

Production deployment also needs security review, DLP validation, monitoring and approved provisioning integration. Do not publish app registration details, secrets, tenant/environment URLs, real email addresses or personal maker account details.
