---
title: Ramp Cards API — Issue Virtual, Physical, and Agent Cards
description: Issue cards that draw against a fund, expose card details safely through the vault or an iframe, and watch spend in real time through the Ramp API.
---

# Cards and Funds

Issue cards, attach controls, and watch spend in real time — virtual or physical, agent-driven or human, embedded in your product or used directly.

A Ramp card is a payment method that draws against a fund. Funds, spend programs, and approvals live in [Spend Controls](./spend-controls.md); this page covers the cards themselves.

## Three card variants

The variant is set at creation. Everything else about a card's behavior is fund and policy configuration.

| Variant | What it is | Use it when |
|---|---|---|
| Virtual | A 16-digit number, CVV, and expiration, always tied to a fund | Online purchases, vendor payments, or surfacing card details inside your own product |
| Physical | A card shipped to an employee, drawing from one or many funds | An employee spends in person — meals, travel, fuel, on-site purchases |
| Agent | A single-use PAN and CVV minted at purchase time, scoped to one merchant | An AI agent needs payment authority constrained to a specific purchase |

Physical cards can auto-match funds by category or carry independent spending restrictions. Agent cards are covered end to end in [Agent Cards](../ai/agent-cards.md).

## Exposing card details

Virtual cards can surface their details two ways, and the choice decides whether a PAN ever touches your infrastructure.

**Browser-side (Ramp-served iframe).** Your backend mints a short-lived token, your frontend loads a Ramp-served iframe, and the iframe renders card details directly to the user. Your servers never see the PAN. Use this when end users inside your product need to read and use card numbers themselves.

**Server-side (Vault API).** Your backend creates a card and retrieves the PAN, CVV, and expiration in one synchronous call. Use this when your service pays vendors directly, automates AP or travel bookings, or executes payments from a backend workflow.

Both patterns share the same issuance step — a fund — and differ only on exposure. You can run both side by side.

### Vault access is gated

Standard API responses return masked card numbers. Full PANs and CVVs require the `cards:read_vault` scope and PCI qualification, and `POST /developer/v1/cards/vault` also requires `limits:write`.

Submit a Developer API support ticket to start qualification. The iframe pattern needs no such approval, and all customers see full card data in [Sandbox](../get-started/sandbox.md).

## Merchants

A merchant is the counterparty on a card transaction — the business where a purchase happened. Merchant identifiers let you filter transactions, and merchant restrictions on a fund let you constrain where a card can spend at all.

Filtering transactions by merchant is one query parameter on the Transactions endpoint. Try it live in the [API Reference](../reference/api-reference.md).

## Scopes you will need

| Operation | Scopes |
|---|---|
| Read cards | `cards:read` |
| Create and manage cards | `cards:write`, often with `users:read` |
| Retrieve full PAN and CVV | `cards:read_vault` plus `limits:write` on vault creation |
| Read the funds a card draws against | `funds:read` |
| Create or update funds | `funds:write` |

See [Authorization](../get-started/authorization.md) for the full scope catalog and least-privilege guidance.

## Next steps

- [Spend Controls](./spend-controls.md) — set the budget and restrictions a card obeys
- [Agent Cards](../ai/agent-cards.md) — give an AI agent single-use payment authority
- [API Reference](../reference/api-reference.md) — send a live request against your own account

## Related

- [Users](./users.md) — the cardholder behind every card.
- [Bill Pay](./bill-pay.md) — paying vendors by card to earn cashback.
