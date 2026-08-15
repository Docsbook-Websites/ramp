---
title: Ramp Bill Pay API — Create, Approve, and Pay Vendor Bills
description: Create approved bills through the Ramp API, pick a payment method, attach documents, and pay vendors end to end without leaving your own stack.
---

# Bill Pay

A bill is the structured accounts payable record in Ramp — the object that powers approvals, coding, and payment. Create approved bills through the API, attach documents, choose a payment method, and pay vendors without forcing your AP team into another tool.

## How a bill works

A bill carries the vendor, amount, due date, line items, and the payment details that fund it. Payments are nested rather than separate: there are no dedicated payment endpoints, so creating a bill includes the payment shape in the same request. `GET /developer/v1/bills` returns payment info as part of the bill object.

## Bill statuses

| Status | Meaning |
|---|---|
| Draft | Invoice uploaded; OCR extracts details |
| Pending approval | Awaiting approver sign-off in Ramp |
| Approved | Ready for payment |
| Paid | Payment processed |

Bills created through the API are automatically approved and enter the workflow at **Approved**. Bills created in the dashboard follow the customer's configured approval policy — see [Spend Controls](./spend-controls.md) for that policy surface.

Draft bills can be created and updated through the API, but they can only be approved in the Ramp dashboard. Approval of a draft bill through the API is not supported.

## Payment methods

Each method carries its own shape on `POST /developer/v1/bills`, and the timing differs enough to change which one you default to.

| Method | What it is | Timing |
|---|---|---|
| Card | Pay by Ramp card, existing or single-use virtual. Earns cashback | Real-time |
| ACH | Bank transfer to a verified vendor bank account | 2–3 business days |
| Check | Mailed check to the vendor's address on file | 5–7 business days |
| Wire | Bank wire transfer | Same-day where supported |

Card payments are the reason to check vendor card eligibility before defaulting to ACH: the same bill paid by card settles immediately and earns cashback.

## Required fields on create

`POST /developer/v1/bills` requires these fields:

| Field | Type | Notes |
|---|---|---|
| `entity_id` | string | The associated business entity |
| `invoice_number` | string | The invoice number on the bill |
| `invoice_currency` | string | ISO 4217 currency code |
| `issued_at` | string | Issued date |
| `due_at` | string | Due date |

Optional fields cover coding and documents: `line_items`, `inventory_line_items`, `accounting_field_selections`, `attachment_id`, `memo`, `payment_date`, and `enable_accounting_sync`.

Set `enable_accounting_sync` to `false` to keep a bill out of the ERP export. See [Conventions](../reference/conventions.md) for how monetary amounts are represented.

## Filtering bills

`GET /developer/v1/bills` accepts filters across identity, status, and time — `vendor_id`, `payment_status`, `approval_status`, `sync_status`, `sync_ready`, `payment_details_missing`, plus date ranges on creation, due date, issue date, payment date, and paid date.

Two filters matter for reconciliation loops: `sync_ready` returns only bills ready to export to the ERP, and `payment_details_missing` surfaces bills that cannot be paid yet because vendor payment details are incomplete.

Try these against your own account in the [API Reference](../reference/api-reference.md).

## Scopes

| Operation | Scopes |
|---|---|
| Read bills and payment history | `bills:read` |
| Create, update, and pay bills | `bills:write` |
| Read vendor payment details | `vendors:read` |
| Create and update vendors | `vendors:write` |

## Next steps

- [Procurement](./procurement.md) — pay against an approved purchase order
- [Spend Controls](./spend-controls.md) — route bills through approvals before payment
- [Webhooks](../reference/webhooks.md) — subscribe to `bills.approved` and `bills.paid`

## Related

- [Cards and Funds](./cards.md) — the single-use virtual cards that pay card-eligible vendors.
- [Sandbox](../get-started/sandbox.md) — simulate a bill payment without moving money.
