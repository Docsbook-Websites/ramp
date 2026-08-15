---
title: Ramp Webhooks — Receive Spend Events Instead of Polling
description: Subscribe to Ramp events, verify signatures, and handle retries so your integration reacts to transactions, bills, and approvals the moment they happen.
---

# Webhooks

Webhooks push notifications to your endpoint whenever an event happens in a Ramp account, so your application reacts in real time instead of polling.

Three things follow: instant notification on events like transactions, approvals, and reimbursements; no polling loop burning rate limit; and downstream automation such as an accounting sync that triggers itself.

## Set up an endpoint

Your endpoint must be publicly accessible, use HTTPS, return a `2xx` status code on receipt, and **process requests within 10 seconds**.

That last requirement decides your architecture. Acknowledge the delivery first and do the real work asynchronously — a handler that calls three APIs before responding will time out under load and force Ramp to retry.

<!-- widget:stepper -->

### Create the subscription

Call the `/developer/v1/webhooks` endpoint with your URL. Your access token needs the scopes for the resources you subscribe to.

### Answer the verification challenge

Ramp sends a `POST` with a challenge string to your endpoint during setup. Receive the challenge and respond with a verification API call.

Ramp also sends a `webhooks.verification` event during this flow. It is not a business-event subscription — do not route it into your event handler.

### Verify signatures on every delivery

Creating a subscription returns a `secret` field. Use it to verify the cryptographic signature Ramp includes with every event, confirming the payload came from Ramp and was not modified in transit.

Validation is optional but recommended for production. On no-code platforms like Zapier, Make, or Pipedream, where custom code is not available, webhooks still work without it.

### Fetch the full resource

The payload carries the affected resource's ID, not the whole object. Use it to fetch details from the matching endpoint — `/transactions/{id}` for transaction events, `/reimbursements/{id}` for reimbursement events.

<!-- /widget -->

## Payload format

Every payload follows the same structure.

| Field | Meaning |
|---|---|
| `id` | Unique event ID, constant across retries — use it to deduplicate |
| `type` | Event type from the tables below |
| `created_at` | Event timestamp in ISO 8601 format |
| `business_id` | Which business the event belongs to |
| `object` | The affected resource ID |

Because `id` stays constant across retries, deduplicating on it is what makes your handler safe to retry.

## Available events

Request the scope for the underlying resource so your integration can read the object after the event arrives.

<!-- widget:accordion -->

### Transactions — `transactions:read`

| Event | Fires when |
|---|---|
| `transactions.authorized` | Transaction has been authorized |
| `transactions.cleared` | Transaction has settled; also fires for refunds and reversals |
| `transactions.declined` | Transaction has been declined |
| `transactions.ready_for_review` | Transaction needs review or coding |
| `transactions.receipt_added` | Receipt has been added to a transaction |
| `transactions.body_coding_updated` | Body-level accounting coding created or changed |
| `transactions.all_requirements_met_and_approved_changed` | Approval and requirements status changed |
| `transactions.ready_to_sync` | Transaction is ready to sync to an accounting system |
| `transactions.sync_requested` | Transaction sync has been requested |
| `transactions.synced` | Transaction has been marked as synced |

### Bills — `bills:read`

| Event | Fires when |
|---|---|
| `bills.created` | Bill has been created |
| `bills.approved` | Bill has been approved |
| `bills.paid` | Bill payment has been completed |
| `bills.rejected` | Bill has been rejected |
| `bills.archived` | Bill has been archived |
| `bills.ready_to_sync` | Bill is ready to sync to an accounting system |
| `bills.updated` | Bill has been edited — invoice number, PO matching, accounting fields, payment details |

`bills.updated` does not fire for approvals or payments. Subscribe to `bills.approved` and `bills.paid` for those.

### Reimbursements — `reimbursements:read`

| Event | Fires when |
|---|---|
| `reimbursements.ready_for_review` | Reimbursement needs review |
| `reimbursements.ready_to_sync` | Ready to sync to an accounting system |
| `reimbursements.sync_requested` | Sync has been requested |
| `reimbursements.batch_payment_reimbursed` | Batch payment has been reimbursed |

### Procurement — `purchase_orders:read`, `unified_requests:read`, `spend_requests:read`

| Event | Fires when |
|---|---|
| `purchase_orders.created` / `.updated` / `.archived` | Purchase order lifecycle |
| `unified_requests.created` / `.updated` / `.modified` | Unified request lifecycle |
| `unified_requests.external_approval_request` | Request is ready for external approval |
| `unified_requests.external_approval_request_reset` | External approval request has been reset |
| `unified_requests.node_advanced` | Request advanced to another workflow node |
| `unified_requests.override_approved` | Request override has been approved |
| `spend_requests.created` / `.comment_created` | Spend request created, or a comment added |

`unified_requests.external_approval_request` is the hook for running approvals in your own system — see [Procurement](../products/procurement.md).

### Vendors and agreements — `vendors:read`

| Event | Fires when |
|---|---|
| `vendors.activated` / `.approved` / `.updated` | Vendor lifecycle |
| `vendor_agreements.created` / `.updated` / `.archived` / `.deleted` | Agreement lifecycle |
| `vendor_agreements.document_added` | Document added to an agreement |
| `vendor_agreements.renewal_milestone` | Renewal milestone reached |

### Users, entities, and other resources

| Event | Scope | Fires when |
|---|---|---|
| `users.invite_accepted` | `users:read` | User accepted an invitation |
| `entities.created` | `entities:read` | Business entity created |
| `item_receipts.created` | `item_receipts:read` | Item receipt created |
| `applications.status_updated` | `applications:read` | Application approved, rejected, or otherwise changed |
| `payments.updated` | No public scope | Bill payment updated |
| `tests.test_event` | None | Mock delivery for testing, not a business subscription |

<!-- /widget -->

## Inbound AI usage events

Ramp also exposes an inbound webhook endpoint for AI platforms that broadcast customer usage into Ramp. If your product lets customers bring their Ramp API key and receive model, token, cost, and attribution data, use `POST /developer/v1/ai-usage/unified`. That endpoint does not require a subscription through `POST /developer/v1/webhooks`.

## Next steps

- [Conventions](./conventions.md) — retry behavior, error shapes, and rate limits
- [API Reference](./api-reference.md) — fetch the full resource behind an event ID
- [Sandbox](../get-started/sandbox.md) — simulate events with the demo actions panel

## Related

- [Bill Pay](../products/bill-pay.md) — the bill lifecycle behind `bills.*` events.
- [Procurement](../products/procurement.md) — routing intake approvals through your own tool.
