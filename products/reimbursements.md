---
title: Ramp Reimbursements API — Pay Employees Back for Expenses
description: Handle out-of-pocket expenses through the Ramp API — manual submissions, OCR receipts, and mileage, gated by funds and synced to your ERP.
---

# Reimbursements

Pay employees back for out-of-pocket expenses — manual submissions, OCR'd receipts, and mileage — gated by funds, routed through approvals, and synced to your ERP.

A reimbursement is an expense an employee paid personally that the business pays back. Submissions enter Ramp from the app, from a receipt upload where Ramp OCRs a draft, or from a mileage post.

## Two parallel state machines

Every reimbursement advances through both at once. Reading only one of them is the usual source of "why has this not exported yet" confusion.

**State** tracks the money: `DRAFT → PENDING (review) → APPROVED → REIMBURSED`.

Edge cases branch off that path: `REJECTED`, `CANCELED`, `FAILED_REIMBURSEMENT`, plus payment-specific states `AWAITING_PAYMENT`, `PROCESSING`, and `REIMBURSED_VIA_PUSH`.

**Sync status** tracks the accounting export: `NOT_SYNC_READY → SYNC_READY → SYNCED`. This is the same accounting pipeline that transactions and bills use, and it drives the ERP export.

A reimbursement can be `APPROVED` and still `NOT_SYNC_READY` — approval releases the payment, coding completeness releases the export.

## Direction

`BUSINESS_TO_USER` is the default: Ramp pays the employee.

`USER_TO_BUSINESS` flows the other way and covers amounts an employee owes the business back.

## Foreign currency reimbursements

For a `BUSINESS_TO_USER` reimbursement in a foreign currency, the original expense amount and the amount actually paid to the employee can use different currencies. Four fields keep them apart.

| Field | Meaning |
|---|---|
| `line_items[].amount` | The original amount for each expense line item |
| `original_reimbursement_amount` | The original total expense amount |
| `amount` and `currency` | The total amount and currency Ramp pays out |

Reconciliation logic that reads only `amount` will disagree with the employee's receipt whenever currencies differ. Read `original_reimbursement_amount` alongside it. See [Conventions](../reference/conventions.md) for how Ramp represents monetary values generally.

## Policy gating

Funds gate which categories and amounts are reimbursable when no card is in play, and approvals route submissions to the right reviewer — Ramp-native or through Blank Canvas. Both live in [Spend Controls](./spend-controls.md).

## Scopes

| Operation | Scopes |
|---|---|
| Read reimbursement requests and status | `reimbursements:read` |
| Read receipt images and data | `receipts:read` |
| Upload and manage receipts | `receipts:write` |
| Read itemized receipt data | `item_receipts:read` |

## Next steps

- [Spend Controls](./spend-controls.md) — set the funds that gate what is reimbursable
- [Users](./users.md) — the employee each reimbursement pays
- [Webhooks](../reference/webhooks.md) — react when a reimbursement is approved or paid

## Related

- [Sandbox](../get-started/sandbox.md) — simulate a reimbursement payment with the demo actions panel.
- [Bill Pay](./bill-pay.md) — the vendor-facing counterpart to employee reimbursements.
