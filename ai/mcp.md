---
title: Ramp MCP Servers — Connect AI Assistants to Ramp Data
description: Three Ramp MCP servers cover account actions, developer documentation, and public datasets. Pick the one that matches what your assistant needs to reach.
---

# MCP Servers

The Model Context Protocol lets AI assistants talk to your systems directly, without copy-paste or context switching. Ramp ships three MCP servers, each scoped to a different audience.

## Which server you want

| Server | URL | Auth | Serves |
|---|---|---|---|
| Ramp MCP | `https://mcp.ramp.com/mcp` | OAuth as a Ramp user | Your account's data and actions |
| Developer MCP | `https://mcp.ramp.com/developer/mcp` | None | Public API documentation and OpenAPI schemas |
| Ramp Data MCP | `https://mcp.ramp.com/ramp-data/mcp` | Provisioned API key | Ramp Rate benchmarks and AI Index metrics |

Sandbox data for Ramp MCP lives at `https://demo-mcp.ramp.com/mcp`.

## Ramp MCP — your account

Ramp MCP connects Ramp to assistants like ChatGPT and Claude so you can query data and take actions in natural language. Admins analyze spend trends and forecasting; employees check balances, request reimbursements, or get policy answers from the tools they already use.

Tools group by what the user is trying to accomplish rather than by tool name, and the surface evolves continuously. Disconnect and reconnect your client to pick up new tools.

<!-- widget:accordion -->

### Read and analyze spend

Search transactions, pull spend exports, query vendors and accounting categories, load departments and entities, and get treasury balances and the org chart.

### Process approvals

Approve or reject transactions, reimbursements, and unified requests such as purchase orders and fund requests. Bill approvals are not yet available through MCP — use the API or the dashboard for those.

### Submit and complete expenses

Employees submit expenses and reimbursements, complete required details, and move their own reimbursement requests forward.

### Edit and act on workflows

Edit transactions (memo, coding, trip), submit reimbursements, post comments, manage cards (lock, unlock, activate), and apply GL coding.

### Answer questions

Policy Q&A, Help Center search, and explanations for declined transactions.

### Make purchases

Generate Agent Card credentials. See [Agent Cards](./agent-cards.md) for the full walkthrough and its restrictions.

<!-- /widget -->

If you manage multiple Ramp businesses, add one MCP connection per business.

## Developer MCP — documentation, no account needed

Developer MCP is Ramp's unauthenticated MCP server for developers building with the API. It gives assistants live access to Ramp's developer documentation, API reference, and OpenAPI schemas inside your coding workflow.

Unlike the account-facing servers, it requires no Ramp account, OAuth login, API key, or developer app. It serves public documentation only.

Ask for an endpoint by operation ID, docs URL, HTTP method and path, or a natural description, and it returns the matching OpenAPI operation with request and response schemas. Use it for authentication setup, pagination, webhooks, required fields, and endpoint behavior without leaving your editor.

Setup in Claude Desktop: open **Settings → Developer**, click **Edit config**, add the Developer MCP server, and restart. In Claude Code, add the server to your session and run `/mcp` to activate it.

## Ramp Data MCP — public benchmarks

Ramp Data MCP exposes Ramp Rate software pricing benchmarks and AI Index business AI adoption metrics. One connection queries both datasets, since the tools live on the same server.

Ramp Data requires provisioned access — apply to the Ramp Data Partner Program for credentials. Set `RAMP_DATA_API_KEY` in your environment, or send your key as `Authorization: Bearer <YOUR_RAMP_DATA_API_KEY>` for any MCP-compatible client.

**Ramp Rate tools** list categories, fetch category summaries, rank vendors within a category, resolve a vendor name to a slug, get a vendor profile, and compare 2–10 vendors.

**AI Index tools** return adoption history overall, by sector, and by company size, each supporting a range of 1 to 120 months.

## Next steps

- [AI Agents on Ramp](./agents.md) — pick between MCP and the CLI for your integration
- [Agent Cards](./agent-cards.md) — add purchasing authority to a connected agent
- [Ramp CLI](./cli.md) — the terminal channel and its skill system

## Related

- [Authorization](../get-started/authorization.md) — how OAuth works for account-facing access.
- [API Reference](../reference/api-reference.md) — the REST surface behind these tools.
