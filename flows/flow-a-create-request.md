# Flow A — Create Request

## Purpose and Trigger

Flow A — Create Request is called by the Request Access topic. It prepares the request, generates CorrelationId, calls Flow H — SummariseCase and creates the Access Requests Dataverse row. In the secure route, manager lookup and a manager-email availability condition precede row creation.

## Inputs From Copilot Studio

The secure topic screenshot shows six inputs:

| Flow input | Secure source | Public/demo contract |
|---|---|---|
| ApplicationName | Captured application | Captured application |
| AccessLevel | Captured choice converted to text | Captured access level |
| Justification | Captured justification | Captured justification |
| DurationDays | Captured duration | Captured duration |
| RequesterEmail | Signed-in email via topic variable | Typed requester email |
| ApproverEmail | Signed-in `User.PrincipalName` | Typed demo approver email |

The public column describes the documented demo contract; the current screenshots verify only the secure topic mappings. They do not establish how the current manager lookup handles a public call. `RequesterEmail` is the visible flow input label; `RequestorEmail` is the documented Dataverse field. `RequestorAadObjectId` is not shown in this six-input call.

## Key Logic

1. Receive structured request details after topic identity and input validation.
2. Evaluate the initial flow condition; its expression is collapsed.
3. On the accepted branch, compose justification, set CorrelationId, call the child flow, assign CaseSummary and compose a request title.
4. Perform manager lookup and evaluate manager-email availability.
5. On the available-manager branch, create the Dataverse row and set row ID, success, Pending status and submitted-message variables.
6. Use CATCH for the pictured error assignments and FINALLY to return values to the agent.

The documented row includes request details, requester/approver identity, SubmittedOn, CaseSummary, CorrelationId and lifecycle fields. Exact create-row mappings, Status Reason and object-ID population are not visible in these collapsed actions.

## Manager Lookup and Missing-Manager Handling

`Get manager (V2)` is visible before `Manager email available`. The secure topic supplies principal name through the existing `ApproverEmail` input, but the lookup's own parameter mapping and the stored approver-address mapping are not expanded.

The available-manager branch contains Create Request Row. The alternate branch contains two variable assignments labelled for failure success/message values. This is a configured missing-email path; the exact condition, assignment values and runtime result remain unverified.

The NotFound run shows a different failure mode: the lookup action fails, the manager-email condition and row creation are skipped, and CATCH executes. It does not prove that a successful lookup returning blank manager email was handled by the alternate branch.

## Outputs and Error Handling

The topic maps six returned values: `issuccess`, `message`, `accessrequestid`, `status`, `casesummary` and `correlationid`. It branches on success to display submission confirmation or a failure message. The exact contents of the returned request identifier are not visible here.

CATCH contains success, error-message and error-status assignments; FINALLY contains the return action. The successful-run continuation shows CATCH skipped and FINALLY succeeded. The NotFound screenshot shows CATCH executing, but not the complete error response returned to the conversation. Error details shown to requesters should be safe and concise.

Flow H has a configured fallback-summary action. Failure/throttling tests are still needed to verify fallback delivery and parent-flow continuation; configuration alone does not prove them.

## Screenshot Evidence

![Create Request manager lookup and create-row branches](../screenshots/createFlow1.png)

Configuration of the accepted-input and manager lookup paths.

![Create Request run with successful manager lookup and row creation](../screenshots/createFlowRunHistorySuccess.png)

Pictured child-flow, lookup and create-row actions have successful run indicators. This does not prove approval or real provisioning.

![Create Request successful return with skipped catch](../screenshots/createFlowRunSuccessReturn.png)

Successful-run continuation: CATCH skipped and FINALLY return succeeded.

![Manager lookup NotFound run enters catch](../screenshots/createFlowManagerNotFound.png)

Lookup NotFound, skipped creation and executed CATCH.

![Create Request catch and finally configuration](../screenshots/createFlowCatchFinally.png)

CATCH/FINALLY structure, with assignment values and run-after settings collapsed.

## Evidence Limits and Outstanding Tests

- Expand trigger inputs, initial validation condition and CorrelationId expression.
- Verify lookup parameters and requester/manager Dataverse mappings; test the public/demo contract separately.
- Exercise successful lookup with missing manager email, plus lookup NotFound, and capture safe agent responses for each.
- Verify optional object-ID and Status Reason mappings if used.
- Test BYOM failure/throttling and parent continuation with fallback.
