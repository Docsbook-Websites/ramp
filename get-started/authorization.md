---
title: Ramp API Authorization — OAuth 2.0 Flows, Scopes, Tokens
description: Choose between client credentials and authorization code, scope your app to least privilege, and manage Ramp access tokens safely in production.
---

# Authorization

Ramp uses OAuth 2.0 for API access, with granular permissions through scopes and two authorization flows for different integration shapes. Ramp authenticates requests to `/developer/v1/token` with your client ID and client secret, typically over HTTP Basic Auth.

For your first call, start with the [Quickstart](./quickstart.md). This page covers the framework in depth.

## Choose a flow

| Flow | Use it for | User consent |
|---|---|---|
| Client Credentials | Internal integrations, server-to-server | None required |
| Authorization Code | Third-party apps, public integrations | Authorized user consent required |

Use **Client Credentials** when your own backend reads or writes your own Ramp data — data sync, internal automation, reporting pipelines. The client secret must stay on your server and never reach a browser or mobile binary.

Use **Authorization Code** when you build for other businesses. The Ramp business grants your app access, and every call is attributed to the consenting account. Partners listing publicly start at [Partner Integrations](./partner-integrations.md).

## How scopes work

Every scope reads as `resource:permission`, where the resource is an API resource such as `transactions`, `bills`, or `users`, and the permission is `read` or `write`.

Request only what your app needs. Read-only first, write added when a flow demands it.

<!-- widget:accordion -->

### Spend and cards

| Scope | Grants |
|---|---|
| `transactions:read` | Transaction data and history |
| `cards:read` | Physical and virtual card information |
| `cards:write` | Create, update, and manage cards |
| `cards:read_vault` | Sensitive card data (PAN, CVV) — requires PCI qualification |
| `limits:write` | Required alongside `cards:read_vault` on `POST /developer/v1/cards/vault` |
| `funds:read` | Funds, members, cards, and spending policies |
| `funds:write` | Create and manage funds, members, and suspension state |
| `spend_programs:read` | Spend program configuration |
| `spend_programs:write` | Create and manage spend programs |
| `merchants:read` | Merchant information and policies |
| `cashbacks:read` | Cashback earnings and history |

### Payables

| Scope | Grants |
|---|---|
| `bills:read` | Bill data and payment history |
| `bills:write` | Create, update, and pay bills |
| `vendors:read` | Vendor information and payment details |
| `vendors:write` | Create and update vendor information |
| `purchase_orders:read` | Purchase order data and status |
| `reimbursements:read` | Reimbursement requests and status |
| `receipts:read` | Receipt images and data |
| `receipts:write` | Upload and manage receipt data |
| `item_receipts:read` | Itemized receipt data |
| `memos:read` / `memos:write` | Read and write transaction memos |

### Organization and accounting

| Scope | Grants |
|---|---|
| `users:read` / `users:write` | User profiles and roles; create and manage accounts |
| `departments:read` / `departments:write` | Department structure and assignments |
| `locations:read` / `locations:write` | Location data and assignments |
| `entities:read` | Entity information and hierarchies |
| `business:read` | Business information and settings |
| `accounting:read` / `accounting:write` | Accounting sync status, settings, and connections |
| `custom_records:read` / `custom_records:write` | Custom data fields and records |
| `statements:read` | Monthly statements |
| `transfers:read` | Transfer history and status |
| `treasury:read` | Banking accounts and balance history |
| `bank_accounts:read` | Connected bank account information |
| `applications:read` / `applications:write` | Financing application status and submission |
| `leads:read` / `leads:write` | Lead and referral information |
| `receipt_integrations:read` / `receipt_integrations:write` | Receipt integration settings |

<!-- /widget -->

These examples are not exhaustive. Check each endpoint's reference entry for the scopes it requires, and configure only those.

## Scope selection rules

Follow least privilege: request the scopes your application actually uses, nothing more. Start read-only and add write access when a feature needs it.

Sensitive scopes carry extra process. `cards:read_vault` requires a security review and PCI qualification before production access.

Some operations need more than one scope. Creating cards can require both `cards:write` and `users:read`.

## Token lifetime and handling

Ramp issues opaque tokens, not JWTs. You cannot decode them for claims.

| Token source | Lifetime |
|---|---|
| Client Credentials | 10 days (864,000 seconds) |
| Authorization Code | 1 hour (3,600 seconds) |
| Refresh Token | 1 hour (3,600 seconds) |

Tokens bind to the scopes they were minted with and cannot exceed them. They also bind to their environment: a sandbox token fails against production.

Treat a token as a credential with full access to its scopes:

- Never log tokens or place them in error messages.
- Store tokens encrypted at rest.
- Use HTTPS for all API communication.
- Rotate tokens in long-running applications.
- Monitor for unusual API usage patterns.

## Next steps

- [Quickstart](./quickstart.md) — mint a token and make the call
- [Sandbox](./sandbox.md) — test flows without moving real money
- [Conventions](../reference/conventions.md) — errors, rate limits, and retry behavior

## Related

- [Partner Integrations](./partner-integrations.md) covers the Authorization Code path end to end.
- [Cards and Funds](../products/cards.md) explains which scopes each card operation needs.
