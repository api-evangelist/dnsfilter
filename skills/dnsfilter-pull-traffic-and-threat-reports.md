---
name: Pull DNSFilter traffic and threat reports
description: Query aggregated DNS query volume and blocked-threat metrics from DNSFilter, broken down by organization, network, agent, or user.
api: openapi/dnsfilter-openapi-original.json
operations:
  - V1_Organizations-index
  - V1_Traffic_Reports-total_requests
  - V1_Traffic_Reports-total_requests_organizations
  - V1_Traffic_Reports-total_threats
  - V1_Traffic_Reports-total_threats_organizations
---

# Pull DNSFilter traffic and threat reports

Retrieve aggregated DNS activity and security metrics for dashboards, SIEM
feeds, or scheduled reporting.

## Auth
Send `Authorization: <api-key>` (raw key, no `Bearer` prefix). Base URL:
`https://api.dnsfilter.com`.

## Steps
1. **Scope to an organization** — `V1_Organizations-index`
   (`GET /v1/organizations`) to get the `organization_id`.
2. **Total DNS requests** — `V1_Traffic_Reports-total_requests`
   (`GET /v1/traffic_reports/total_requests`) for overall query volume; use
   `V1_Traffic_Reports-total_requests_organizations` /
   `_agents` / `_users` for breakdowns.
3. **Threats blocked** — `V1_Traffic_Reports-total_threats`
   (`GET /v1/traffic_reports/total_threats`), with
   `_organizations` for the per-org breakdown.
4. **Filter by time window** using the report query parameters, and paginate
   large result sets with `page[number]` / `page[size]`.

## Conventions
- Responses use the `data` / `meta` envelope.
- On `429`, honor `Retry-After` and `RateLimit-*` headers.
- Errors: `{ "error": "..." }` (V1) — see errors/dnsfilter-problem-types.yml.
