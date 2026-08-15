---
title: Ramp Agent Cards — Single-Use Payment Authority for AI Agents
description: Mint a single-use PAN and CVV scoped to one merchant and capped by a fund, so an AI agent can buy under policy you set and log every purchase.
---

# Agent Cards

Give an AI agent real payment authority on Ramp — a single-use PAN and CVV, scoped to one merchant and capped at the requested amount. It works with agents connected through an MCP runtime (Claude Desktop, ChatGPT, Perplexity) or the Ramp CLI (Claude Code, Codex, and other terminal agents).

By the end of this page you will have an agent that picks a fund, mints a single-use card, checks out at a real merchant, and closes the loop on the receipt, memo, and coding — without leaving its chat window.

## Three pieces make it work

**A single-use credential.** Ramp mints a fresh PAN and CVV at purchase time, scoped to one merchant and capped at the requested amount. It expires after the first authorization or 12 hours, whichever comes first.

**A Ramp fund** sets the policy ceiling. The agent can only generate credentials against funds it is scoped to. See [Spend Controls](../products/spend-controls.md).

**An agent connected to Ramp**, through an MCP runtime or the CLI's `agentic-purchase` skill. Both honor your existing Ramp permissions.

The credential is the security boundary. An agent that is compromised or confused still cannot spend past the fund's ceiling or at a merchant outside the one it was scoped to.

## Prerequisites

A Ramp account with at least one active fund, and an AI agent connected through an MCP runtime or the Ramp CLI. Enrolling your business in Agent Cards is a one-time setup step covered by the Agentic Purchase playbook.

Agent Card issuance is policy-controlled at the business level. If your use case needs a higher ceiling than the default, submit a Developer API support ticket.

## Required scopes

Agent Card endpoints require `cards:read_agentic`. Writes need `spend_limits:write`.

If you authenticated as a user without those scopes, ask a Ramp admin to grant access. On the CLI, run `ramp update` first — older versions surface less helpful scope errors.

## Running in production

If your agent runs through the Ramp CLI on a remote host, two patterns matter. MCP-runtime sessions are managed by the connector instead — see [AI Agents on Ramp](./agents.md).

Authenticate locally, then copy the session config to wherever the agent runs. This is what most production users do today. As long as the agent makes at least one call inside the refresh window, the session keeps rolling.

| Session type | Expires |
|---|---|
| Read-only | One week after last use |
| Read-write | 24 hours after last use |

**What breaks this:** if your agent runtime rewrites `~/.config/ramp/config.toml` on restart — some hosted agent platforms do — the refresh token is destroyed and the agent loses auth on the next start. Persist the config file outside the rewrite scope, or restore it from a backup at startup.

Treat `config.toml` like a credential. Never commit it to source control.

## Restrictions

Agent purchases cannot run against these Merchant Category Codes.

| MCC | Category |
|---|---|
| 4816 | Computer Network / Information Services |
| 5122 | Drugs, Drug Proprietors and Druggists Sundries |
| 5816 | Digital Goods: Games |
| 5912 | Drug Stores and Pharmacies |
| 5966 | Direct Marketing – Outbound Telemarketing Merchant |
| 5967 | Direct Marketing – Inbound Telemarketing Merchants |
| 5968 | Direct Marketing – Continuity / Subscription Merchant |
| 5993 | Cigar Stores and Stands |
| 6012 | Member Financial Institution – Merchandise and Services |
| 6051 | Quasi Cash – Merchant |
| 6211 | Securities – Brokers and Dealers |
| 7273 | Dating Services |
| 7995 | Betting, lottery tickets, casino gaming chips, off-track betting, wagers at race tracks |

Two flows are not yet supported, both on the roadmap:

**3D Secure (3DS)** affects online transactions in the EEA, UK, India, Japan, Australia, and others.

**Merchant-initiated transactions and card on file** affect subscriptions, recurring billing, and metered AI spend. Plan around this if your use case is a recurring charge rather than a one-time purchase.

## Troubleshooting

<!-- widget:accordion -->

### I can't find an Agent Cards page in the dashboard

There isn't one. The credential is generated at purchase time and shows up as a regular transaction on the linked fund. The audit log entry attributes the transaction to the agent acting on your behalf.

### The merchant declined the card, and its MCC is not on the blocklist

Some bill-payment aggregators have known acceptance issues with single-use credentials — Paymentus is the most common report. The MCC list covers network-level blocks; submit a support ticket for declines outside it.

### Checkout fails on the billing address

Credentials do not carry a default billing address. Pass it explicitly in the prompt, for example: "use Ramp to renew my domain, billing address 123 Main St, San Francisco, CA 94110."

### The agent keeps losing authentication inside the refresh window

The likely cause is your agent runtime rewriting `~/.config/ramp/config.toml` on restart. Persist the file outside the rewrite scope, or restore it at startup.

### Scope errors on Agent Card endpoints

Agent Card endpoints need `cards:read_agentic`, and writes need `spend_limits:write`. Ask a Ramp admin to grant them. On the CLI, run `ramp update` first.

<!-- /widget -->

## Next steps

- [Spend Controls](../products/spend-controls.md) — set the fund ceiling an agent can never exceed
- [Ramp CLI](./cli.md) — the command surface and the `agentic-purchase` skill
- [AI Agents on Ramp](./agents.md) — the broader capability surface across MCP and CLI

## Related

- [Cards and Funds](../products/cards.md) — how agent cards compare to virtual and physical cards.
- [Authorization](../get-started/authorization.md) — scoping an agent integration to least privilege.
