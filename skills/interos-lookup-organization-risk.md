---
name: Look up an organization and read its risk profile
description: Find an organization in the Interos graph by name, then retrieve its full risk profile.
api: openapi/interos-openapi.json
operations:
  - get_organizations_search_lite_v1_external_organizations_search_lite_get
  - get_organization_profile_v1_external_organizations__organization_id__get
---

# Look up an organization and read its risk profile

Use the Interos API (`https://api.interos.ai`) to resolve a company name to an
Interos `organization_id` and then pull its risk profile.

## Authentication
Every request needs two headers (see `authentication/interos-authentication.yml`):
- `x-api-key`: your active developer key
- `x-customer-id`: your customer/tenant id

Optionally call `GET /health` first to confirm the key is active.

## Steps
1. **Search for the organization.** `GET /v1/organizations/search_lite` with
   `search_term=<company name>` and optional `country_code`. Read the result list and
   pick the `organization_id` of the best match.
2. **Fetch the profile.** `GET /v1/organizations/{organization_id}` with
   `include_risk_variables=true` and `include_attributes=true` (and `detail_level`)
   to get the `risk_profile`, `risk_score`, industry codes, `ultimate_parent_*`, and
   identifiers (`primary_duns_number`, `primary_cage_code`).

## Conventions & errors
- List responses use the cursor envelope `{data, next, total_count}`; pass `limit` and
  the returned `next` cursor to page (never send `next` on the first request).
- A `404` means the organization is not visible to this customer; `403` means the
  key/customer lacks access; `422` is a parameter validation error
  (`detail[].loc` / `detail[].msg`). See `errors/interos-problem-types.yml`.
