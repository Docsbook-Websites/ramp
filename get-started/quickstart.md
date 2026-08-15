---
title: Ramp API Quickstart — Your First Call in Five Minutes
description: Create a developer app, mint an access token with client credentials, and pull real transaction data from the Ramp API in five minutes.
---

# Quickstart

Make your first Ramp API call in about five minutes. You will create an access token and call the Transactions API to retrieve transaction data.

This guide assumes you are a Ramp customer with admin access to your dashboard. Need a test environment instead? See [Sandbox](./sandbox.md).

<!-- widget:stepper -->

### Create a developer app

In your Ramp account, go to **Company → Developer**. Admin access is required.

1. Click **Create New App**.
2. Name your app, for example `API Quickstart`, and accept the Terms.
3. Under **Grant types**, click **Add new grant type** and select **Client Credentials**.
4. Under **Scopes**, click **Configure allowed scopes** and select `transactions:read`.
5. Copy your **Client ID** and **Client Secret**.

Store both securely. You need them in the next step, and the secret is shown once.

### Request an access token

Ramp authenticates the token request with HTTP Basic Auth: base64-encode `client_id:client_secret` and send it in the `Authorization` header.

```bash
curl -X POST https://api.ramp.com/developer/v1/token \
  -H "Authorization: Basic $(echo -n 'YOUR_CLIENT_ID:YOUR_CLIENT_SECRET' | base64)" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "scope=transactions:read"
```

The response carries the token you will send on every later call.

```json
{
  "access_token": "YOUR_ACCESS_TOKEN",
  "token_type": "Bearer",
  "expires_in": 864000,
  "scope": "transactions:read"
}
```

Client credentials tokens live for 10 days. Authorization Code and Refresh Token flows issue one-hour tokens instead — see [Authorization](./authorization.md).

### Call the Transactions API

Send the token as a bearer credential.

```bash
curl https://api.ramp.com/developer/v1/transactions \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

You get a page of transactions plus a `next` cursor when more results exist. Follow that cursor to walk the full set — see [Conventions](../reference/conventions.md) for the pagination contract.

<!-- /widget -->

## Pick the right base URL

The API host differs from the host people log in to. Pointing a call at the app host is the most common first-run error.

| Environment | API base URL | Ramp app URL |
|---|---|---|
| Production | `https://api.ramp.com` | `https://app.ramp.com` |
| Sandbox (demo) | `https://demo-api.ramp.com` | `https://demo.ramp.com` |

Tokens are bound to the environment that issued them. A sandbox token returns an authorization error against production.

## Troubleshooting

<!-- widget:accordion -->

### 401 Unauthorized on the token request

Your Basic Auth header is malformed or the client secret is wrong. Re-encode `client_id:client_secret` with no trailing newline — `echo -n` matters. Confirm the app has the **Client Credentials** grant type enabled.

### 401 on the transactions call, but the token request worked

You are sending the token to the wrong environment, or the token expired. Check the base URL against the table above, and mint a fresh token if more than 10 days passed.

### 403 with a scope error

The app does not hold `transactions:read`. Add the scope in **Company → Developer**, then request a new token — existing tokens keep the scopes they were minted with.

### 429 Too Many Requests

You crossed 200 requests in a 10-second rolling window. Back off exponentially — 1s, 2s, 4s — before retrying. See [Conventions](../reference/conventions.md) for the full limit.

<!-- /widget -->

## Next steps

- [Authorization](./authorization.md) — pick the OAuth flow that matches your integration and scope it down
- [Sandbox](./sandbox.md) — build against demo data with simulated payments
- [API Reference](../reference/api-reference.md) — send live requests from the browser

<!-- widget:cta -->

**Keep going**

## Issue your first card

Transactions are read-only. Funds and cards are where the API starts moving money under your policy.

[Read Cards and Funds](../products/cards.md) · [Get started free](https://ramp.com/signup)

<!-- /widget -->
