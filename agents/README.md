# Agent Design

This folder documents the Copilot Studio agent design for the Access Request & Approval Agent.

## Agent Variants

| Variant | Purpose |
|---|---|
| Access Request Agent (Public) | Public/demo mode for low-friction portfolio review |
| Access Request Agent (Internal Auth / Secure) | Secure/enterprise mode using Entra ID identity capture |
| Access Request (Secure) | Secure variant label used for authenticated demonstration |

Both variants use the same core lifecycle idea: the agent gathers request details and calls Power Automate, while Dataverse and watcher flows control approval, resolution, fulfilment, SLA monitoring, and auditability.

## Core Topics

- Request Access
- GetStatus / Check Request Status

## Design Principles

- The agent collects and validates input.
- The agent does not approve or reject access.
- The agent does not perform fulfilment.
- Approval, resolution, SLA monitoring, and fulfilment logic stay in Power Automate for reliability, run history, retry control, Idempotency, Error capture, and Dataverse write-back.

## Public Safety

Do not publish exported bot configuration if it contains tenant-specific URLs, environment URLs, authentication settings, secrets, connection data, or private identifiers.
