---
title: Ramp Partner Integrations — Build, List, and Keep 100% Revenue
description: Build an integration on the Ramp API, pass production review, and get listed in front of 50,000+ businesses while keeping all of your revenue.
---

# Partner Integrations

Ramp lets technology partners build integrations that reach 50,000+ businesses. This page covers what each side owns, the four stages to a public listing, and how pricing works.

Building for your own company instead? Start with the [Quickstart](./quickstart.md) and use the Client Credentials flow.

## Why partners build on Ramp

**You keep 100% of your revenue.** Set your own pricing, charge customers directly, and grow without revenue sharing or platform fees. Ramp does not process payments or participate in billing.

**Your integration gets a public listing.** Once published, it appears on the Ramp Integrations page, which carries visibility and credibility with prospective customers.

## Who owns what

Building an integration is a partnership aimed at a high-quality experience for shared customers. The split is explicit.

| Your side | Ramp's side |
|---|---|
| Pricing and monetization — you bill customers directly and keep all revenue | API troubleshooting and advisory — debugging help and best-practice guidance |
| Building and hosting the integration on Ramp's APIs | Product review and guidance — approval is required before production access |
| Customer support through your own channels, with a public support email | Distribution to 50,000+ businesses through the Integrations page |
| Documentation — setup guides, workflows, troubleshooting, hosted by you | |
| Marketing assets — logo, descriptions, screenshots, videos for the listing | |

You own the product. Ramp provides oversight, guidance, and reach.

## The four stages

<!-- widget:stepper -->

### Build

Develop against the sandbox using the Authorization Code flow, since your app acts on behalf of other businesses rather than your own account. Scope the app to least privilege — see [Authorization](./authorization.md).

Accounting and ERP integrations carry extra requirements around two-way sync. Review the ERP guidance before you design the sync loop.

### Apply for production access

Submit your integration for Ramp product review. The team reviews your flows and returns feedback. Approval gates production access, so build the review into your timeline rather than treating it as a formality at the end.

### Beta testing

Run with real customers on production credentials under a limited rollout. Use this stage to find the failure modes that sandbox data hides — rate limits under real volume, partial ERP syncs, webhook retries.

### Apply for a public listing

Submit your marketing assets and documentation for the Ramp Integrations page. Sales, customer success, and support teams at Ramp use your documentation when they discuss your integration, so its quality affects how often you get recommended.

<!-- /widget -->

## Pricing your integration

You control pricing. Ramp charges no fees and takes no revenue share. Many integrations are free; others charge. That choice is yours.

Price relative to the value the integration delivers. Keep the model predictable and easy to understand, and factor in implementation cost, ongoing maintenance, and support burden.

Common models:

- Flat annual subscription.
- Implementation fee plus annual renewal.
- Usage-based pricing, for example per transaction or per bill.
- Tiered feature bundles based on functionality.

## Documentation expectations

High-quality documentation improves customer activation and reduces support load. Ramp's own sales, customer success, and support teams lean on it when discussing your integration with customers.

Host setup guides, workflow instructions, and troubleshooting on your own site, and keep them current with your release cycle.

## Next steps

- [Authorization](./authorization.md) — implement the Authorization Code flow your listing requires
- [Sandbox](./sandbox.md) — build and test before production review
- [Webhooks](../reference/webhooks.md) — react to customer events instead of polling

<!-- widget:cta -->

**For technology partners**

## Put your integration in front of 50,000+ businesses

Build on the sandbox, pass review, and list publicly while keeping every dollar you charge.

[Talk to Ramp](https://ramp.com/see-a-demo) · [Start with the quickstart](./quickstart.md)

<!-- /widget -->
