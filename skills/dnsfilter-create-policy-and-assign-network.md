---
name: Create a filtering policy and assign it to a network
description: Create a DNSFilter content-filtering policy and apply it to a network (site) so DNS resolution is filtered per your rules.
api: openapi/dnsfilter-openapi-original.json
operations:
  - V1_Organizations-index
  - V1_Policies-create
  - V1_Policies-show
  - V1_Networks-create
  - V1_Networks-update
---

# Create a filtering policy and assign it to a network

Use the DNSFilter management API to define a filtering policy and attach it to a
network (a site whose DNS traffic is filtered).

## Auth
Send every request with `Authorization: <api-key>` (the raw key, no `Bearer`
prefix). Create/obtain a key in the dashboard under Account Settings or via the
Api Keys API. Base URL: `https://api.dnsfilter.com`.

## Steps
1. **Resolve your organization** — `V1_Organizations-index` (`GET /v1/organizations`)
   to get the `organization_id` you are operating in.
2. **Create the policy** — `V1_Policies-create` (`POST /v1/policies`) with a
   `name`, `organization_id`, and rules: `blacklist_categories`,
   `whitelist_domains` / `blacklist_domains`, `allow_unknown_domains`, and
   safe-search toggles (`google_safesearch`, `youtube_restricted`, etc.).
3. **Verify** — `V1_Policies-show` (`GET /v1/policies/{id}`) to confirm the
   policy was created with your settings.
4. **Create or select a network** — `V1_Networks-create` (`POST /v1/networks`)
   or list existing networks. Note the network `id`.
5. **Assign the policy** — `V1_Networks-update` (`PATCH /v1/networks/{id}`) to
   bind the policy (via the network's policy assignment fields, e.g.
   `allow_all_policies` / scheduled policy) so the network filters accordingly.

## Conventions
- Pagination on list endpoints: `page[number]` / `page[size]`.
- On `429`, honor `Retry-After` and the `RateLimit-*` headers.
- Errors return `{ "error": "..." }` (V1) — see errors/dnsfilter-problem-types.yml.
