---
name: Monitor organizations on a scenario watchlist
description: List the customer's organization groups and read the organizations flagged on a scenario watchlist.
api: openapi/interos-openapi.json
operations:
  - get_all_for_customer_v1_external_organization_groups_get
  - get_watchlist_organizations_scenarios_watchlists__watchlist_id__organizations_get
---

# Monitor organizations on a scenario watchlist

Use the Interos API (`https://api.interos.ai`) to review organizations that scenarios
have flagged onto a watchlist.

## Authentication
Send `x-api-key` and `x-customer-id` on every request
(see `authentication/interos-authentication.yml`).

## Steps
1. **List the customer's groups (optional context).** `GET /v1/organization_groups`
   with `limit` / `next` to page. Use `return_count=true` for `total_count`.
2. **Read watchlist organizations.** `GET /v1/scenarios/watchlists/{watchlist_id}/organizations`.
   Filter with `scenario_ids`, `match_all_scenarios`, `status`, `scen_activity`,
   `organization_ids`, or `search_term`. Page with `limit` + `next`.

## Conventions & errors
- Cursor envelope `{data, next, total_count}` applies; continue while `next` is set.
- `404` means the watchlist is not visible to this customer; `422` signals invalid
  filter parameters (`detail[].loc` / `detail[].msg`). See
  `errors/interos-problem-types.yml`.
