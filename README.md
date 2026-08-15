---
title: Ramp Developer Docs — Build Spend Automation on Ramp
description: Issue cards, pay bills, sync your ERP, and give AI agents real payment authority through the Ramp Developer API. Make your first call in five minutes.
---

# Build on Ramp

Ramp is the finance platform that 70,000+ businesses run their spend on. The Developer API opens that platform to your code: issue cards against enforced budgets, create and pay bills, sync a customer's ERP two ways, and give AI agents payment authority that respects your policy.

Every endpoint runs on the same policy engine as the Ramp dashboard. A fund's limit holds whether the spend comes from a person, a card, or an agent.

<!-- widget:cards -->

## Start here

- [Quickstart](./get-started/quickstart.md) — Create credentials and call the Transactions API in five minutes {rocket}
- [Authorization](./get-started/authorization.md) — OAuth 2.0 flows, scopes, and token lifetime {key-round}
- [Sandbox](./get-started/sandbox.md) — Build against demo data before you touch production {flask-conical}
- [Partner Integrations](./get-started/partner-integrations.md) — List your integration in front of 50,000+ businesses {handshake}

## Products

- [Cards and Funds](./products/cards.md) — Virtual, physical, and agent cards, each drawing against a fund {credit-card}
- [Spend Controls](./products/spend-controls.md) — Funds, spend programs, and approvals: the policy layer {shield-check}
- [Bill Pay](./products/bill-pay.md) — Bills, payment methods, approvals, and 3-way match {file-text}
- [Reimbursements](./products/reimbursements.md) — Out-of-pocket expenses, OCR receipts, and mileage {receipt}
- [Users](./products/users.md) — Provision, deactivate, and pre-stage the people who spend {users}
- [Procurement](./products/procurement.md) — Intake requests, approvals, and purchase orders {clipboard-check}

## Build with AI agents

- [AI Agents on Ramp](./ai/agents.md) — Give an agent read access and action authority via MCP or the CLI {bot}
- [Agent Cards](./ai/agent-cards.md) — Single-use credentials scoped to one merchant and capped by you {sparkles}
- [MCP Servers](./ai/mcp.md) — Three servers, each scoped to a different audience {plug}
- [Ramp CLI](./ai/cli.md) — OAuth, expenses, bills, and travel from a terminal or agent loop {terminal}

## Reference

- [API Reference](./reference/api-reference.md) — Try live calls against Transactions, Users, and Bills {code}
- [Conventions](./reference/conventions.md) — Pagination, monetary values, deferred tasks, and errors {book-open}
- [Webhooks](./reference/webhooks.md) — Subscribe to events instead of polling {webhook}

<!-- /widget -->

## What you can build

Ramp's API surface covers 170 endpoints across nine product areas. These are the journeys developers start with.

| Goal | Where to start | Typical time |
|---|---|---|
| Read transaction data into a warehouse or dashboard | [Quickstart](./get-started/quickstart.md) | 5 minutes |
| Issue virtual cards inside your own product | [Cards and Funds](./products/cards.md) | 1–3 hours |
| Pay vendors end to end without leaving your stack | [Bill Pay](./products/bill-pay.md) | Half a day |
| Sync a customer's chart of accounts and export spend | [Procurement](./products/procurement.md) | Multi-day |
| Let an AI agent make a real purchase under policy | [Agent Cards](./ai/agent-cards.md) | 30–60 minutes |

## How Ramp fits together

Four objects explain most of the API. Funds hold the budget and its restrictions. Cards draw against funds. Users are the principal behind every spend object. Approvals decide what passes before money moves.

Everything else — bills, reimbursements, purchase orders, agent purchases — composes those four. Learn them once in [Spend Controls](./products/spend-controls.md) and the rest of the surface reads as variations.

<!-- widget:cta -->

**Free to start**

## Make your first API call

Create a developer app in the Ramp dashboard, mint a token, and pull real transactions.

[Start the quickstart](./get-started/quickstart.md) · [Get started free](https://ramp.com/signup)

<!-- /widget -->
