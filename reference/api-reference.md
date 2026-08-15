---
title: Ramp API Reference — Try Live Calls in Your Browser
description: Send real requests to the Ramp Developer API from this page — mint a token, list transactions, read users, and filter bills with your own credentials.
---

# API Reference

Send live requests against the Ramp Developer API from this page. Paste a bearer token into the Authorization field, fill in the parameters you want, and read the real response.

The canonical schema is the [OpenAPI specification](https://docs.ramp.com/openapi/developer-api.json), covering 170 endpoints. The endpoints below are the ones most integrations start with.

Base URLs: `https://api.ramp.com` for production, `https://demo-api.ramp.com` for [sandbox](../get-started/sandbox.md). Need a token first? See the [Quickstart](../get-started/quickstart.md).

<!-- widget:api -->

## POST /developer/v1/token

Mint an access token. Send `client_id:client_secret` base64-encoded in the Authorization header as HTTP Basic Auth.

| Field | Type | Required | Description |
|---|---|---|---|
| `grant_type` | string | yes | One of `authorization_code`, `client_credentials`, or `refresh_token` |
| `scope` | string | no | Space-separated list of scopes to grant the returned token |
| `code` | string | no | Authorization code, for the `authorization_code` flow |
| `redirect_uri` | string | no | The redirect URI used in the authorization request |
| `refresh_token` | string | no | The refresh token issued to you, to get a new access token |

### Example

```bash
curl -X POST https://api.ramp.com/developer/v1/token \
  -H "Authorization: Basic $(echo -n 'YOUR_CLIENT_ID:YOUR_CLIENT_SECRET' | base64)" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials" \
  -d "scope=transactions:read"
```

### Response

```json
{
  "access_token": "YOUR_ACCESS_TOKEN",
  "token_type": "Bearer",
  "expires_in": 864000,
  "scope": "transactions:read"
}
```

## GET /developer/v1/transactions

List transactions with filters across merchant, user, amount, date, and sync state. Requires `transactions:read`.

| Field | Type | Required | Description |
|---|---|---|---|
| `from_date` | string | no | Transactions with a `user_transaction_time` after this date |
| `to_date` | string | no | Transactions with a `user_transaction_time` before this date |
| `merchant_id` | string | no | Filter by merchant |
| `user_id` | string | no | Filter by user |
| `card_id` | string | no | Filter by physical card |
| `spend_program_id` | string | no | Filter by spend program |
| `department_id` | string | no | Filter by department |
| `entity_id` | string | no | Filter by business entity |
| `state` | string | no | Filter by transaction state; `ALL` includes every state |
| `min_amount` | number | no | Transactions with an amount larger than this value |
| `max_amount` | number | no | Transactions with an amount smaller than this value |
| `has_been_approved` | boolean | no | Only approved, or only unapproved, transactions |
| `requires_memo` | boolean | no | Transactions that require a memo but do not have one |
| `sync_status` | string | no | Filter by sync status; supersedes `sync_ready` when set |
| `include_merchant_data` | boolean | no | Include all purchase data provided by the merchant |
| `order_by_date_desc` | boolean | no | Sort by transaction date, newest first |
| `page_size` | integer | no | Results per page |
| `start` | string | no | ID of the last entity on the previous page, used as the cursor |

### Example

```bash
curl "https://api.ramp.com/developer/v1/transactions?page_size=10&order_by_date_desc=true" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## GET /developer/v1/users

List the people on the account. Requires `users:read`.

| Field | Type | Required | Description |
|---|---|---|---|
| `email` | string | no | Filter by email address |
| `status` | string | no | `USER_DRAFT`, `INVITE_PENDING`, `USER_ACTIVE`, `USER_INACTIVE`, `USER_SUSPENDED` |
| `role` | string | no | Filter by Ramp role |
| `department_id` | string | no | Filter by department |
| `entity_id` | string | no | Filter by business entity |
| `employee_id` | string | no | Filter by your own employee identifier |
| `page_size` | integer | no | Results per page |
| `start` | string | no | Pagination cursor |

### Example

```bash
curl "https://api.ramp.com/developer/v1/users?status=USER_ACTIVE&page_size=25" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

## GET /developer/v1/bills

List bills with filters across vendor, status, and dates. Requires `bills:read`.

| Field | Type | Required | Description |
|---|---|---|---|
| `vendor_id` | string | no | Filter by vendor |
| `entity_id` | string | no | Filter by business entity |
| `invoice_number` | string | no | Exact invoice number |
| `payment_status` | string | no | Filter by payment status |
| `approval_status` | string | no | Bill approval status, separate from payment status |
| `payment_method` | string | no | Filter by payment method |
| `sync_ready` | boolean | no | Only bills ready to sync to the ERP |
| `payment_details_missing` | boolean | no | Only bills missing payment details |
| `is_archived` | boolean | no | Show archived bills instead of active ones |
| `from_due_date` | string | no | Bills due on or after this date |
| `to_due_date` | string | no | Bills due on or before this date |
| `min_amount` | number | no | Bills at or above this amount |
| `max_amount` | number | no | Bills at or below this amount |
| `page_size` | integer | no | Results per page |
| `start` | string | no | Pagination cursor |

### Example

```bash
curl "https://api.ramp.com/developer/v1/bills?sync_ready=true&page_size=50" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Errors

| Status | Meaning |
|---|---|
| `401` | Missing, expired, or wrong-environment token |
| `403` | The token's scopes do not cover this endpoint |
| `429` | Rate limit exceeded — 200 requests per 10-second window |
| `504` | Request exceeded the 60-second timeout; paginate into smaller pages |

<!-- /widget -->

## Related

- [Conventions](./conventions.md) — pagination, monetary values, deferred tasks, and error shapes.
- [Authorization](../get-started/authorization.md) — the scope each endpoint requires.
- [Webhooks](./webhooks.md) — event delivery instead of polling these endpoints.
