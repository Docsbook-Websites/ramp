---
title: Ramp Spend Controls — Funds, Spend Programs, and Approvals
description: Compose funds, spend programs, and approvals into policy that holds across cards, bills, reimbursements, and AI agent purchases on Ramp.
---

# Spend Controls

The policy layer behind every dollar spent on Ramp — budgets, templates, and approval workflows that apply equally to cards, reimbursements, bills, and agent purchases.

Three primitives compose every spend control. They are independent objects with their own APIs, designed to plug into each other.

## The three primitives

**Funds.** A budget with restrictions: amount, interval (daily, monthly, total), allowed merchant categories, allowed merchants, allowed countries. A fund issues a virtual card automatically and can attach to physical cards. Funds also gate reimbursements when no card is involved.

**Spend programs.** Templates that mass-produce funds with consistent policy. Reach for one when the same budget shape recurs — quarterly offsites, per-employee SaaS allowances, contractor stipends, monthly travel funds. Mint a fund from a program and the policy comes with it.

**Approvals.** The review path a spend action travels before it is allowed. Approvals route bills, reimbursements, purchase orders, and fund-creation requests through configured policy. Reviews stay Ramp-native or hop to your own system through Blank Canvas approvals.

## Compose, don't pick

A spend control is rarely a single primitive.

| Real-world control | Composition |
|---|---|
| Team travel budget | Spend program template producing funds, plus approvals above a threshold |
| Vendor SaaS allowance | A single fund, no template |
| $50K capex approval | An approvals workflow with no budget object yet |

Start with the object that matches what recurs. If the same policy shape repeats across people or quarters, that is a spend program. If it happens once, a bare fund is enough.

## Where controls apply

Spend controls cut across every product that puts money in motion.

| Surface | What spend controls do |
|---|---|
| [Cards](./cards.md) | Funds define the budget every virtual card draws against; multiple cards can share one fund |
| [Reimbursements](./reimbursements.md) | Funds gate which categories and amounts are reimbursable when no card is in play |
| [Bill Pay](./bill-pay.md) | Approvals route bills to the right reviewer before payment |
| [Procurement](./procurement.md) | Approvals gate intake requests and purchase orders |
| [Agent Cards](../ai/agent-cards.md) | A fund sets the ceiling an agent can never exceed |

That last row is the reason the policy layer matters more with agents than without. An agent generates credentials only against funds it is scoped to, so the fund is the enforcement point, not the agent's own judgment.

## Scopes

| Operation | Scopes |
|---|---|
| Read funds, members, cards, and policies | `funds:read` |
| Create and manage funds, members, suspension state | `funds:write` |
| Read spend program configuration | `spend_programs:read` |
| Create and manage spend programs | `spend_programs:write` |

## Next steps

- [Cards and Funds](./cards.md) — issue the cards that draw against these budgets
- [Procurement](./procurement.md) — route intake and purchase orders through approvals
- [Webhooks](../reference/webhooks.md) — react when spend crosses a policy boundary

## Related

- [Authorization](../get-started/authorization.md) — scope your app for fund and program access.
- [AI Agents on Ramp](../ai/agents.md) — how policy holds when an agent is the spender.
