---
name: Create, publish and cover shifts (Shyft)
description: List shifts, create a shift at a location, publish it to the team, and let a worker cover an open shift using the Shyft Customer API schedule_elements endpoints.
api: openapi/shyft-customer-openapi-original.json
operations:
  - "GET /api/customer/user_role/schedule_elements"
  - "POST /api/customer/user_role/locations/{location_id}/schedule_elements"
  - "PUT /api/customer/user_role/schedule_elements/{shift_id}/publish"
  - "PUT /api/customer/user_role/schedule_elements/{shift_id}/cover"
---

# Create, publish and cover shifts

In Shyft a shift is a `schedule_element`. Managers create and publish shifts;
workers cover open shifts or apply. Authenticate first
(see `shyft-phone-authentication.md`).

## Steps

1. **List shifts**: `GET /api/customer/user_role/schedule_elements`
   (or `/schedule_elements/calendar` for the mobile calendar view). Paginate with `per_page`.
2. **Create a shift** at a location:
   `POST /api/customer/user_role/locations/{location_id}/schedule_elements`
   (bulk: `/schedule_elements/bulk_create`).
3. **Publish it** so the team can see it:
   `PUT /api/customer/user_role/schedule_elements/{shift_id}/publish`
   (bulk: `/schedule_elements/bulk_publish`; reverse with `/unpublish`).
4. **Cover / assign**: a worker covers with
   `PUT /api/customer/user_role/schedule_elements/{shift_id}/cover`; a manager can
   `/pick_applicant`, `/approve`, or `/reject`.

## Rules
- Draft-first is supported (`/draft_create`, `/draft_update`) before publishing.
- Destructive actions (`DELETE .../schedule_elements/{shift_id}`, `/call_off`,
  `/revoke`) are not idempotent — no idempotency-key mechanism exists, so do not
  blind-retry. See `conventions/shyft-conventions.yml` and `errors/shyft-problem-types.yml`.
