---
name: Authenticate a user by phone number (Shyft)
description: Sign a Shyft user up / in via phone number, retrieve the SMS verification code, and obtain the Bearer JWT used for all subsequent Customer API calls.
api: openapi/shyft-customer-openapi-original.json
operations:
  - "POST /api/users"
  - "GET /api/code_auth/{phone_number}"
  - "POST /api/code_auth/{phone_number}"
  - "POST /api/users/sign_in"
---

# Authenticate a Shyft user by phone number

The Shyft Customer API secures every non-public endpoint with an HTTP **Bearer JWT**
(`Authorization: Bearer <token>`). Public auth endpoints use the `noauth` scheme.
Most authenticated calls also carry a `Session-Uukey` header. All requests/responses
are JSON; responses include an `x-request-id` header for correlation.

## Steps

1. **Sign the user up** (new users): `POST /api/users` with the phone number.
2. **Request the SMS code**: `GET /api/code_auth/{phone_number}`. In a test context
   this returns the verification code directly (see `sandbox/shyft-sandbox.yml`).
3. **Verify the code**: `POST /api/code_auth/{phone_number}` with the SMS code, or
   **sign in / confirm**: `POST /api/users/sign_in`. The response contains the JWT.
4. **Use the token**: send `Authorization: Bearer <jwt>` (plus `Session-Uukey`) on
   every `/api/customer/...` request.

## Rules
- A `401` means the token is missing/expired — re-run sign-in. A `403` means the
  authenticated user lacks the role (`user_role` vs `admin_role`) for that resource.
- `422` carries field-level validation errors (see `errors/shyft-problem-types.yml`).
- Never invent SMS codes or tokens; only the code-auth helper returns test codes.
