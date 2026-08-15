---
title: Ramp Sandbox — Test Integrations Without Moving Money
description: Build and test Ramp API integrations against demo data, simulate payments with the demo actions panel, and promote to production with confidence.
---

# Sandbox

The Ramp sandbox is a testing environment where developers and partners build, test, and experiment with the API without touching live production data. No real money moves in sandbox — demo accounts cannot process actual financial transactions.

## What sandbox gives you

- A safe place to test integrations before you deploy to production.
- Full Developer API access for building custom integrations.
- Simulated events, so you can drive a bill to `Paid` or a reimbursement to `REIMBURSED` without a real payment rail.
- Full card data. Every customer sees complete PANs and CVVs in sandbox, where production gates them behind PCI qualification.

## Sandbox hosts

| Resource | URL | Description |
|---|---|---|
| Ramp frontend | `https://demo.ramp.com` | The Ramp user interface for testing |
| API | `https://demo-api.ramp.com` | Base URL for API calls in sandbox |

Tokens are environment-bound. Mint sandbox tokens against `demo-api.ramp.com` — a production token fails here, and the reverse fails too.

## Simulate events with the demo actions panel

While building against the developer API, simulation actions let you drive state transitions that would otherwise wait on a real payment network. Open the demo actions panel anywhere in the Ramp sandbox with `⌘ J`.

<!-- widget:accordion -->

### Mark a bill as paid

Navigate to Bill Pay in the sandbox UI and open the bill you want to pay. Press `⌘ J` and select **pay current bill**.

This action depends on your Bill Pay payment release setting. When release is enabled, it works only for bills that are approved and scheduled. When disabled, it works for all bills.

### Mark a reimbursement as paid

Open the reimbursements tab and select **needs review** to find pending reimbursements. Open the one you want to advance, then press `⌘ J` and pick the payment simulation action.

Use this to exercise the `APPROVED → REIMBURSED` transition and any webhook handler listening for it.

<!-- /widget -->

## Get sandbox access

Sandbox is available to Ramp customers and to developers who do not yet have a Ramp account. Request access through the Ramp developer support flow, then create a developer app inside the sandbox dashboard exactly as you would in production — see [Quickstart](./quickstart.md).

## Promote to production

When your integration works in sandbox, three things change:

1. **Base URL** moves from `demo-api.ramp.com` to `api.ramp.com`.
2. **Credentials** are re-created in the production dashboard. Sandbox client IDs do not carry over.
3. **Gated scopes** apply. `cards:read_vault` needs PCI qualification in production even though sandbox returned full card data freely.

Test your retry and error handling in sandbox before promoting. See [Conventions](../reference/conventions.md) for the error shapes and rate limits you will meet in production.

## Next steps

- [Quickstart](./quickstart.md) — create credentials and make a first call
- [Authorization](./authorization.md) — scope the app you will promote
- [Webhooks](../reference/webhooks.md) — verify signatures against simulated events

## Related

- [Bill Pay](../products/bill-pay.md) — the bill statuses the demo panel advances.
- [Reimbursements](../products/reimbursements.md) — the reimbursement state machine you can simulate.
