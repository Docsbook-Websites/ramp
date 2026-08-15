---
title: Ramp Users API — Provision, Deactivate, and Pre-Stage People
description: Provision Ramp users from your HRIS or IdP, poll the deferred creation task, and move people through the lifecycle without breaking spend history.
---

# Users

The people on a Ramp account — provisioned, updated, and deactivated through the Users API. Users are the principal behind every spend object.

Every transaction, card, reimbursement, and approval traces back to a user. Provisioning and deprovisioning are the touchpoints between Ramp and your HRIS, IdP, or onboarding tool.

## User statuses

The `status` field tells you where a person sits in the lifecycle.

| Status | Meaning |
|---|---|
| `USER_DRAFT` | The record exists but no invite was sent. They cannot log in or spend. Use it to pre-stage new hires starting next month |
| `INVITE_PENDING` | An invite email went out; the user has not accepted it yet |
| `USER_ACTIVE` | The user accepted the invite, is onboarded, and can use Ramp |
| `USER_INACTIVE` | Deactivated. Cards are frozen and spend is blocked, but the record is preserved for history and reactivation |
| `USER_SUSPENDED` | Ramp suspended the user for risk or policy reasons. Rare, and API callers cannot set it |

Deactivation preserves history rather than deleting it. That is what keeps past transactions attributable after someone leaves.

## User creation is asynchronous

Creating a user does not return a user. `POST /developer/v1/users/deferred` returns a task id, and you poll for the result.

<!-- widget:stepper -->

### Submit the creation request

Call `POST /developer/v1/users/deferred` with the user's details. The response carries a task id, not a user id.

### Poll the task status

Call `GET /developer/v1/users/deferred/status/{task_id}` every one to two seconds. Status values are `STARTED`, `IN_PROGRESS`, `ERROR`, and `SUCCESS`. Most tasks finish in under five seconds.

### Read the user id

On `SUCCESS`, `data.user_id` holds the UUID of the created user. Store it — every later card, reimbursement, or approval references it.

<!-- /widget -->

This polling pattern is shared across the API. See deferred tasks in [Conventions](../reference/conventions.md).

## Filtering users

`GET /developer/v1/users` accepts `employee_id`, `role`, `status`, `email`, `department_id`, and `entity_id`, plus `start` and `page_size` for pagination.

Filtering by `status` is how a nightly HRIS sync finds people to deactivate without walking the whole directory. Try the call in the [API Reference](../reference/api-reference.md).

## Scopes

| Operation | Scopes |
|---|---|
| Read user profiles and roles | `users:read` |
| Create and manage user accounts | `users:write` |
| Read department structure | `departments:read` |
| Read entity hierarchies | `entities:read` |

Card creation often needs `users:read` alongside `cards:write`, since a card binds to a cardholder.

## Next steps

- [Cards and Funds](./cards.md) — issue a card to a newly provisioned user
- [Spend Controls](./spend-controls.md) — attach the policy that governs their spend
- [Webhooks](../reference/webhooks.md) — react to lifecycle changes instead of polling on a schedule

## Related

- [Authorization](../get-started/authorization.md) — scoping an HRIS sync to least privilege.
- [Reimbursements](./reimbursements.md) — the expenses a user submits.
