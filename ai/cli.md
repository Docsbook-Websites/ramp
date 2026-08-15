---
title: Ramp CLI — Manage Spend from Your Terminal or Agent Loop
description: Authenticate with OAuth, approve bills, submit expenses, and drive agent purchases from the terminal, with JSON output built for scripted workflows.
---

# Ramp CLI

Authenticate with OAuth, manage expenses, approve bills, book travel, and more from your terminal or AI agent. The CLI is open source at `github.com/ramp-public/ramp-cli`.

## Install and authenticate

The install script detects your platform, downloads a pre-built binary, and sets up the `ramp` command. If you already use `uv`, install through it instead.

Authenticate with a browser-based OAuth flow:

```bash
ramp auth login
```

The CLI connects to **Sandbox by default**. Switch with the `--env` flag or set a default:

```bash
ramp env set production
```

## Actions run as you

Every CLI action runs as the user who authenticated. Three consequences follow:

**Actions are attributed to you.** Approving a bill, submitting a reimbursement, or posting a comment shows your name in Ramp, exactly as if you did it in the dashboard.

**Permissions match your Ramp role.** `ramp transactions list --transactions_to_retrieve my_transactions` returns your own transactions; `all_transactions_across_entire_business` requires admin access. If you cannot see it in the dashboard, you cannot see it in the CLI.

**Data visibility follows your access.** Non-admins see only their own transactions, bills, and reimbursements. Admins query across the business.

Run `ramp users me` to confirm which user the CLI is acting as.

## Output modes

In a terminal, output defaults to human-readable tables. When piped, it switches to JSON automatically — which is what makes the CLI usable inside an agent loop without extra flags.

| Flag | Behavior |
|---|---|
| `--human` | Human-readable table output (default in a terminal) |
| `--agent` | Machine-readable JSON output (default when piped) |
| `--wide` | Show all columns in table output |

## Commands and resources

Commands handle setup and configuration. Resources map to API entities, each with its own tools. Usage is `ramp <resource> <tool> [OPTIONS]`.

<!-- widget:accordion -->

### Commands

| Command | Description |
|---|---|
| `auth` | Login, logout, check status |
| `config` | Get and set CLI configuration |
| `env` | Show or set default environment (sandbox/production) |
| `applications` | Apply for a Ramp account |
| `skills` | Browse and install agent skill instructions |
| `feedback` | Submit feedback about the CLI |

### Resources

| Resource | Tools |
|---|---|
| `accounting` | categories, category-options |
| `bills` | search, get, draft, pending, approve, attachments |
| `funds` | list, activate, creds, lock |
| `general` | comment, explain, help-center, policy |
| `purchase_orders` | search, get |
| `receipts` | upload, attach |
| `reimbursements` | list, pending, submit, approve, edit |
| `requests` | pending, approve |
| `transactions` | list, get, approve, edit, missing, flag-missing, explain-missing, memo-suggestions, trips |
| `travel` | list, create, bookings, locations |
| `users` | me, search, org-chart |

Run `ramp <resource> <tool> --help` for the flags on any tool.

### Global flags

| Flag | Values |
|---|---|
| `--env`, `-e` | `sandbox` (default) or `production` |
| `--output`, `-o` | `json` or `table` |
| `--quiet`, `-q` | Suppress progress output |
| `--no-input` | Disable interactive prompts, for CI and scripts |

Per-tool flags follow common patterns: `--json TEXT` passes a raw JSON request body and bypasses flags, `--dry_run` prints the request without sending it, and `--page_size` with `--next_page_cursor` walks pagination.

<!-- /widget -->

For non-interactive environments, combine `--no-input` with `--quiet`.

## Skills for agent frameworks

The CLI includes a skill system for AI agent frameworks. Skills are structured instructions that teach an agent how to use Ramp for a specific workflow — approving expenses, uploading receipts, or making purchases.

The `agentic-purchase` skill drives [Agent Cards](./agent-cards.md) end to end: request the credential, check out, and close the audit loop. Skills can call MCP tools underneath, so an agent with CLI access also reaches the full MCP surface.

Browse and install them with `ramp skills`.

## Next steps

- [Agent Cards](./agent-cards.md) — the purchase flow the `agentic-purchase` skill drives
- [AI Agents on Ramp](./agents.md) — choosing between the CLI and MCP
- [MCP Servers](./mcp.md) — the tool surface the CLI shares

## Related

- [Sandbox](../get-started/sandbox.md) — the environment the CLI targets by default.
- [Authorization](../get-started/authorization.md) — how the OAuth session behind `ramp auth login` works.
