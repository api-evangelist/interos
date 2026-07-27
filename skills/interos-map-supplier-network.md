---
name: Map an organization's supplier network
description: Walk an organization's suppliers by tier and its multi-hop supplier connections in the Interos relationship graph.
api: openapi/interos-openapi.json
operations:
  - get_organization_profile_v1_external_organizations__organization_id__get
  - get_suppliers_for_organization_v1_external_organizations__organization_id__suppliers_get
  - get_suppliers_for_organization_v1_external_organizations__destination_organization_id__connections_get
---

# Map an organization's supplier network

Use the Interos API (`https://api.interos.ai`) to expand an organization into its
supplier graph.

## Authentication
Send `x-api-key` and `x-customer-id` on every request
(see `authentication/interos-authentication.yml`).

## Steps
1. **Confirm the anchor organization.** `GET /v1/organizations/{organization_id}` to
   verify the target and read its `ultimate_parent_id`.
2. **List direct suppliers by tier.** `GET /v1/organizations/{organization_id}/suppliers`
   with the `tier` query param. Page with `limit` + `next`; set `return_count=true` for
   `total_count`.
3. **Traverse deeper connections.** `GET /v1/organizations/{destination_organization_id}/connections`
   with `path_length` to follow multi-hop supplier relationships in the graph.

## Conventions & errors
- All list endpoints return `{data, next, total_count}`; loop while `next` is present.
- Optional flags `enable_restrictions_update` and `enable_iscore_update` refresh
  restriction/iScore data on read.
- Handle `404` (org not found for this customer) and `422` (invalid `tier`/`path_length`)
  per `errors/interos-problem-types.yml`.
