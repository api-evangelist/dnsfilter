---
name: Manage DNSFilter API keys
description: Programmatically create, list, and revoke DNSFilter API keys for automation and integrations.
api: openapi/dnsfilter-openapi-original.json
operations:
  - V1_Api_Keys-create
  - V1_Api_Keys-index
  - V1_Api_Keys-show
  - V1_Api_Keys-revoke
  - V1_Api_Keys-destroy
---

# Manage DNSFilter API keys

Provision and rotate the API keys that authenticate DNSFilter API access.

## Auth
Send `Authorization: <api-key>` (raw key, no `Bearer` prefix) on every request.
Base URL: `https://api.dnsfilter.com`.

## Steps
1. **List existing keys** — `V1_Api_Keys-index` (`GET /v1/api_keys`). Each key
   exposes `name`, `expiry`, `last_four`, `status`, and timestamps (never the
   full secret after creation).
2. **Create a key** — `V1_Api_Keys-create` (`POST /v1/api_keys`) with a `name`
   (and optional `expiry`). The full `token` is returned **once** on creation —
   store it securely; you cannot retrieve it again.
3. **Inspect a key** — `V1_Api_Keys-show` (`GET /v1/api_keys/{id}`).
4. **Rotate / revoke** — `V1_Api_Keys-revoke` (`POST /v1/api_keys/{id}/revoke`)
   to invalidate a key, or `V1_Api_Keys-destroy` (`DELETE /v1/api_keys/{id}`)
   to delete it. Rotate by creating the new key first, cutting traffic over,
   then revoking the old one.

## Conventions
- On `429`, honor `Retry-After` and `RateLimit-*` headers.
- Errors: `{ "error": "..." }` (V1) — see errors/dnsfilter-problem-types.yml.
