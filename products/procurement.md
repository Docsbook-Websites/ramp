---
title: Ramp Procurement API — Intake, Approvals, Purchase Orders
description: Bring Ramp intake requests and approvals into your own tool, return decisions through Blank Canvas, and read purchase orders once they are approved.
---

# Procurement

Bring procurement requests, approvals, and purchase orders into your stack — so the system that already drives intake or approvals stays the source of truth.

Ramp Procurement is the path from "an employee needs to buy something" to an approved purchase order.

## Three concepts

**Intake.** A request, typically a custom form attached to a spend program, capturing what is being bought, the amount, the vendor, and any policy answers.

**Approvals.** The review path the request travels. Reviews can be Ramp-native or routed to an external system through Blank Canvas approvals. [Spend Controls](./spend-controls.md) covers the approvals surface across Bill Pay, Reimbursements, and Procurement.

**Purchase orders.** The artifact created once a request is approved and ready to be paid.

## Integration surfaces

Each concept has a place where your system plugs in.

| You want to | Surface |
|---|---|
| Receive requests as they are submitted | Intake requests delivered as webhooks |
| Run the review in your own tool | Blank Canvas approvals — return the decision to Ramp |
| Read approved purchase orders | Purchase orders endpoints, `purchase_orders:read` |
| Read intake and contract requests | Unified Requests |
| Read submitted form answers | Custom Forms |

The design point is that Ramp does not insist on being the reviewer. If your procurement or contract management system already owns approvals, Ramp sends the request out and takes the decision back.

## Where procurement connects

| Neighbor | Relationship |
|---|---|
| [Bill Pay](./bill-pay.md) | The payment side of an approved PO, and home of the vendor an intake request resolves against |
| [Spend Controls](./spend-controls.md) | Funds, spend programs, and the approval workflows that gate intake |
| [AI Agents](../ai/agents.md) | Agents can approve or reject unified requests — POs and fund requests — through MCP |

That third row is worth pausing on: an agent approving a purchase order is the same approvals object a human would act on, with the same audit trail.

## Scopes

| Operation | Scopes |
|---|---|
| Read purchase order data and status | `purchase_orders:read` |
| Read vendor information | `vendors:read` |
| Read spend program configuration | `spend_programs:read` |
| Read custom records and forms | `custom_records:read` |

## Next steps

- [Bill Pay](./bill-pay.md) — pay against an approved purchase order
- [Webhooks](../reference/webhooks.md) — receive intake requests as they arrive
- [Spend Controls](./spend-controls.md) — configure the approval path a request travels

## Related

- [AI Agents on Ramp](../ai/agents.md) — letting an agent act on approval queues.
- [Authorization](../get-started/authorization.md) — the scopes a procurement integration needs.
