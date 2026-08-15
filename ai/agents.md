---
title: AI Agents on Ramp — MCP, CLI, and Real Payment Authority
description: Give an AI agent read access, action authority, and approved purchasing power on Ramp through MCP or the CLI, with every action in the audit log.
---

# AI Agents on Ramp

Give an AI agent read access, action authority, and — with approval — real purchasing power on Ramp, through MCP, the Ramp CLI, or both. Every action respects the authenticated user's role and lands in the Ramp audit log.

![Ramp agents introduction panel from the Ramp Intelligence page](https://ramp.com/assets/images/intelligence/meet-the-agents.webp)

## Pick a channel

Agents reach Ramp through two channels. Both honor the authenticated user's permissions and write every action to the audit log.

| | MCP | Ramp CLI |
|---|---|---|
| Best for | Off-the-shelf assistants (Claude, ChatGPT, Cursor, Claude Code) and any MCP-compatible tool | Coding agents, or any agent with terminal access, and scripted or scheduled workflows |
| Setup | One-click in supported clients; custom config elsewhere | `curl ... \| sh`, then `ramp auth login` |
| Surface | Tool calls dispatched by the AI client | Commands and skills invoked by the agent |
| Auth | Browser OAuth on first use | Browser OAuth on first use; session reusable on remote hosts |
| Capability fit | Conversational analysis, approvals, edits | Scripted reads and writes, agent-driven purchases |

The channels overlap — both call Ramp MCP tools. The CLI adds skills that compose tool calls into a workflow. Most teams pick one and add the other when a specific capability requires it.

**Estimated time:** 5–15 minutes to a first agent action through MCP; 30–60 minutes to a working agent purchase through [Agent Cards](./agent-cards.md).

## Prerequisites

Any agent integration needs a Ramp production or [sandbox](../get-started/sandbox.md) account and access to a supported client.

Production MCP embedded in your own product, or behind a custom client, also needs redirect-URI allowlisting through Ramp support. Technology partners building a product that connects customers' Ramp accounts apply through [Partner Integrations](../get-started/partner-integrations.md).

[Agent Cards](./agent-cards.md) additionally require at least one active fund.

## Pattern A: connect through MCP

Ramp ships an MCP server at `https://mcp.ramp.com/mcp`, with demo data at `https://demo-mcp.ramp.com/mcp`.

The simplest integration is a supported client — one click in the client's directory, or a single command in a terminal client. Other MCP-compatible tools connect with the same server URL. If you manage multiple Ramp businesses, add one MCP connection per business. See [MCP Servers](./mcp.md) for the full setup matrix.

<!-- widget:accordion -->

### External MCP clients must use the shared OAuth client

External MCP clients are supported, but you authorize through the shared Ramp MCP OAuth client, not through your own Developer API OAuth application.

### Redirect URIs need allowlisting

Embedding Ramp MCP in your own product, or routing through a gateway, requires Ramp to allowlist the redirect URI first.

Use `https://` or `localhost`/`127.0.0.1` only. The exact host is required — wildcard subdomains are not supported. Submit a Developer API support ticket with your client name and redirect URI.

### Microsoft Copilot for 365

Copilot for 365 does not natively support remote MCP servers. Integrate through Copilot Studio's MCP server integration, or use the Developer API directly through Power Automate.

<!-- /widget -->

## Pattern B: connect through the CLI

The CLI fits when the agent has terminal access or runs as a script or scheduled job — coding agents, CI workflows, headless services on a remote host.

The CLI exposes packaged skills an agent can invoke. The most common one for AI-driven flows is `agentic-purchase`, which drives Agent Cards end to end: request credential, checkout, audit.

Skills can call MCP tools underneath, so an agent with CLI access also has the full MCP capability surface. The CLI is built on `https://api.ramp.com/agent-tools` endpoints, which are not accessible to external clients directly.

For remote hosts, authenticate locally and copy the session config — see [Ramp CLI](./cli.md).

## What an agent can actually do

Capabilities group by what the agent does for the user rather than by tool name. The MCP tool surface evolves continuously — disconnect and reconnect your client to pick up new tools as they ship.

Reads cover searching transactions by merchant, amount, date, or user; full spend exports with SQL-style analysis for admins; and vendor lists, accounting categories, departments, entities, and treasury balances.

Actions cover approvals, edits, and — through Agent Cards — purchases constrained by a fund.

## Next steps

- [Agent Cards](./agent-cards.md) — give an agent single-use payment authority under a fund ceiling
- [MCP Servers](./mcp.md) — the three servers and which audience each serves
- [Ramp CLI](./cli.md) — commands, output modes, and session handling

<!-- widget:cta -->

**Built for finance teams**

## Connect an agent to your Ramp account

Start read-only through MCP, then add purchasing authority when the policy is set.

[Set up Agent Cards](./agent-cards.md) · [Get started free](https://ramp.com/signup)

<!-- /widget -->
