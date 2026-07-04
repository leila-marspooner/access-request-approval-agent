# Flow C — Resolution Watcher

## Purpose

Flow C — Resolution Watcher processes approved and rejected outcomes after Flow B — Approval Watcher writes the decision back to Dataverse.

## Trigger

Dataverse row modified.

## Approved Path

- Check FulfillmentStarted and/or FulfilledOn.
- Mark FulfillmentStarted where used.
- Send simulated access-granted notification.
- Set Status to Fulfilled.
- Stamp FulfilledOn.
- Add FulfillmentNotes.

## Rejected Path

- Notify requester of rejection.
- Include ApproverComments where appropriate.
- Do not trigger fulfilment.

## Important Limitation

Fulfilment/provisioning is mocked or simulated in this portfolio project unless real provisioning evidence is added later.

## Error Capture

TRY/CATCH should write failures to FulfillmentError.
