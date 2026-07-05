# Future Improvements

These items are realistic next steps for the **Access Request & Approval Agent**. They should not be described as built unless implementation evidence is added.

## Provisioning

- Replace mocked/simulated fulfilment with real provisioning.
- Integrate with Microsoft Graph for Entra ID group assignment.
- Add application-role assignment where appropriate.
- Integrate with an ITSM or identity governance platform for enterprise workflows.
- Add rollback/removal logic for time-bound access.

## Identity And Approval Routing

- Validate ApproverEmail against Entra ID.
- Resolve managers automatically through Microsoft Graph.
- Add delegated approvals for out-of-office scenarios.
- Add duplicate request detection.
- Add requester cancellation or withdrawal flow.

## Governance And Security

- Add DLP policy evidence.
- Add Managed Environment evidence.
- Add deeper Field Security Profile coverage where required.
- Add service-owned connection evidence for non-dev environments.
- Add monitoring alerts for failed flows and overdue requests.

## Reporting

- Add Power BI reporting over Dataverse.
- Track request volume, approval time, resolution time, rejection reasons, reminder volume, and SLA breach trends.
- Add dashboard screenshots after redaction.

## ALM And Testing

- Add deployment pipeline notes.
- Add repeatable test data strategy.
- Add release notes for each MAJOR.MINOR.PATCH.BUILD version.
- Add managed solution import/export evidence after redaction.
- Add automated flow test evidence where practical.

## Agent Experience

- Add richer requester-friendly status messages.
- Add a cancellation topic.
- Add clearer secure-mode messaging.
- Add a canvas app request form option for users who prefer forms over chat.

## Architecture

- Add a production-hardening diagram showing Graph provisioning, monitoring, service accounts, and DLP boundaries.
- Add optional integration points for ITSM and Entra ID Governance without claiming replacement.
