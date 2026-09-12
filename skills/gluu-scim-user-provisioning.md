---
name: gluu-scim-user-provisioning
description: Provision, update and deprovision users and groups in Gluu Flex / Janssen Server over SCIM 2.0.
api: gluu:jans-scim-api
spec: openapi/gluu-jans-scim-openapi.yml
operations:
  - get-users
  - create-user
  - search-user
  - get-user-by-id
  - update-user-by-id
  - patch-user-by-id
  - delete-user-by-id
  - get-groups
  - create-group
  - patch-group-by-id
  - get-service-provider-config
generated: '2026-09-12'
method: generated
source: openapi/gluu-jans-scim-openapi.yml + conventions/gluu-conventions.yml + errors/gluu-problem-types.yml
---

# Provision users over SCIM 2.0

The SCIM service runs on the customer's own deployment at `https://{host}/jans-scim/restv1/v2`. Every
call needs an OAuth 2.0 access token from that deployment's Auth Server carrying the SCIM scopes (the
spec applies a `scim_oauth` security scheme). Bodies and responses are `application/scim+json`.

## Before you write anything

1. Call **`get-service-provider-config`** (`GET /ServiceProviderConfig`). It tells you which optional
   SCIM features this deployment actually supports — patch, bulk, filtering, sort, ETag — and the
   maximum bulk payload. Do not assume; deployments differ.
2. There is **no idempotency mechanism** on this API. A retried `create-user` creates a second user.
   Always search before you create.

## Create a user

1. **Search first.** `search-user` (`POST /Users/.search`) with a `SearchRequest` body, or
   `get-users` (`GET /Users`) with `filter=userName eq "jdoe"`. Both are safer than a blind create.
2. **Create.** `create-user` (`POST /Users`) with
   `schemas: ["urn:ietf:params:scim:schemas:core:2.0:User"]`, `userName`, `name`, `emails`, `active`.
   Set **`externalId`** to the identifier your own system owns — it is the join key you will need later,
   and the server-assigned `id` is not one you can predict.
3. The response carries `meta.location`; use the `id` from it for every later call.

## Update a user

- **`patch-user-by-id`** (`PATCH /Users/{id}`) with an RFC 7644 §3.5.2 PatchOp body is the correct verb
  for a partial change. Use it for anything you did not read first.
- **`update-user-by-id`** (`PUT /Users/{id}`) replaces the whole resource. Only use it when you have just
  read the resource and are writing back a complete representation — a PUT built from partial data
  silently clears attributes you omitted.

## Group membership

Membership is written on the **group**, not on the user. `UserResource.groups[]` is read-only per
RFC 7643. To add someone to a group, `patch-group-by-id` (`PATCH /Groups/{id}`) with an `add` operation
on `members`.

## Deprovision

`delete-user-by-id` (`DELETE /Users/{id}`) is **immediate and permanent**. There is no soft delete, no
trash, and no restore operation anywhere in this API. Treat it as the irreversible action on this
platform: require explicit confirmation, and prefer setting `active: false` via `patch-user-by-id` when
what you actually want is to disable access.

To revoke a user's live sessions rather than the account, use `revoke-tokens` (`DELETE /UserTokens`).

## Errors

Failures come back as the RFC 7644 §3.12 SCIM error envelope — `status` (a string), `scimType`,
`detail` — not RFC 9457 problem+json. Branch on `scimType` for machine handling. A `401` means the token
is missing, expired or lacks the SCIM scope; a `409` on create means the `userName` is taken.

## Bulk

`POST /Bulk` applies many operations in one request (RFC 7644 §3.7), bounded by the limits in
`get-service-provider-config`. It is **not transactional** — members can fail individually, so read the
per-operation status in the response rather than the HTTP status alone.
