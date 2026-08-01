---
name: Read and set a worker's availability (Shyft)
description: Fetch a location's availabilities and submit or update the signed-in worker's current availability using the Shyft Customer API user_role endpoints.
api: openapi/shyft-customer-openapi-original.json
operations:
  - "GET /api/customer/user_role/locations/{location_id}/availabilities/mine_current"
  - "GET /api/customer/user_role/locations/{location_id}/availabilities/all"
  - "POST /api/customer/user_role/locations/{location_id}/availabilities"
  - "PUT /api/customer/user_role/locations/{location_id}/availabilities/{availability_id}"
---

# Manage worker availability

Availability is scoped to a **location** and gated by role. As a worker use the
`user_role` path; managers use the parallel `admin_role`/`availability_requests`
endpoints. Authenticate first (see `shyft-phone-authentication.md`).

## Steps

1. **Read your current availability**:
   `GET /api/customer/user_role/locations/{location_id}/availabilities/mine_current`.
2. **See the team's availability** (optional):
   `GET /api/customer/user_role/locations/{location_id}/availabilities/all`.
3. **Submit new availability**:
   `POST /api/customer/user_role/locations/{location_id}/availabilities`.
4. **Update an existing entry**:
   `PUT /api/customer/user_role/locations/{location_id}/availabilities/{availability_id}`.
   Use `response[put_return_resource]` to control whether the updated resource is echoed.

## Rules
- Paginate list endpoints with `per_page` (page-based; no cursors).
- A `403` indicates you used a `user_role` path but the action needs `admin_role`
  (or vice-versa). `422` returns validation details. See `conventions/shyft-conventions.yml`.
