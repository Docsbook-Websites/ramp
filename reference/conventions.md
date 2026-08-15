---
title: Ramp API Conventions — Pagination, Money, Errors, Retries
description: The cross-cutting rules every Ramp endpoint follows — keyset pagination, canonical monetary amounts, deferred tasks, error shapes, and rate limits.
---

# Conventions

API rules and cross-cutting patterns. These apply across every endpoint, so learning them once removes most of the surprises from the rest of the surface.

## Pagination

Ramp uses keyset pagination, which relies on columns with unique, ordered values rather than numbered offsets.

Request the first page. The response includes a `next` field holding the complete URL for the following page. Send that URL, which carries the `start` query parameter as a cursor. Repeat until `next` comes back `null` — that means you have every record.

Two properties follow from keyset rather than offset pagination: results stay stable while new records are inserted mid-walk, and you cannot jump to "page 7" without walking there.

## Monetary values

Monetary values pair a `currency_code` with an `amount`. Currency codes are three-letter ISO 4217 acronyms, such as `USD`.

**Canonical representation is preferred.** It uses the `CurrencyAmount` type to group `amount` and `currency_code` together, with the amount as an integer in the currency's smallest denomination — which avoids floating-point precision loss.

The amount is the value multiplied by 10 raised to the power of D, where D is the number of decimal places for that currency.

| Currency | Example amount (canonical) | Decimal places |
|---|---|---|
| USD | `4000` = $40.00 | 2 |
| EUR | `10000` = €100.00 | 2 |
| GBP | `2500` = £25.00 | 2 |
| JPY | `5000` = ¥5000 | 0 |

JPY is the case that breaks naive code: dividing every amount by 100 turns ¥5000 into ¥50. Read D from the currency rather than assuming two decimals.

A legacy representation still exists and remains supported for backward compatibility. Write new integrations against the canonical form.

## Deferred tasks

Some calls require asynchronous processing and are marked with `/deferred/` in the request URL. Creating a user is the common example.

These endpoints return immediately with a task ID. Poll `GET .../deferred/status/{id}` to follow it.

| Field | Meaning |
|---|---|
| `status` | One of `STARTED`, `IN_PROGRESS`, `ERROR`, `SUCCESS` |
| `data` | Details about the result; on a successful creation it carries the new object's ID |

Poll every one to two seconds. Most tasks finish in under five seconds. See [Users](../products/users.md) for the creation flow end to end.

## Errors

Ramp returns detailed error information in the response body alongside standard HTTP status codes.

**Unknown fields are ignored.** Extra query parameters and extra body fields do not cause errors, and requests succeed as long as required fields are present and valid. This gives forward compatibility, but it also means a misspelled field name fails silently instead of erroring. Validate requests against the schema during development.

Handle errors in this order:

1. **Read the status code** to identify the category — 4xx for client errors, 5xx for server errors.
2. **Parse the `error_v2` field**, using `error_code` and `additional_info` to pinpoint the issue and give your users something actionable.
3. **Log the response**, including the `x-trace-id` header, so support can correlate it.
4. **Retry transient failures** — 429 and 5xx — with exponential backoff.

## Rate limits and timeouts

The default limit is **200 requests per 10-second rolling window**, applied per source IP address. The window is rolling, so each new request resets the timer for that request.

Crossing it returns `429 Too Many Requests`. Back off exponentially — 1s, 2s, 4s — rather than retrying immediately, which extends the rolling window and makes the delay worse.

Requests running longer than **60 seconds** are terminated with `504 Gateway Timeout`. Paginate large reads into smaller pages and retry 504s with backoff.

If your application needs a higher limit, document your expected usage and submit a Developer API support ticket.

## Trace IDs and support

Every API response carries an `x-trace-id` header. It correlates logs, metrics, and the response, showing how the request was processed.

Log it in your application. Include it whenever you contact Ramp support — it is the difference between a fast diagnosis and a slow one.

## Next steps

- [API Reference](./api-reference.md) — send live calls that follow these conventions
- [Webhooks](./webhooks.md) — avoid the rate limit entirely by not polling
- [Quickstart](../get-started/quickstart.md) — a first call with pagination in place

## Related

- [Users](../products/users.md) — the deferred creation pattern in practice.
- [Reimbursements](../products/reimbursements.md) — where currency precision matters most.
