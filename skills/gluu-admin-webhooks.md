---
name: gluu-admin-webhooks
description: Register, map and trigger Gluu Flex Admin UI webhooks over the Janssen Config API.
api: gluu:jans-config-api
spec: openapi/gluu-jans-config-api-admin-ui-plugin-openapi.yml
operations:
  - get-all-features
  - get-all-webhooks
  - post-webhook
  - put-webhook
  - get-features-by-webhook-id
  - get-webhooks-by-feature-id
  - trigger-webhook
  - delete-Webhook-by-inum
generated: '2026-09-12'
method: generated
source: openapi/gluu-jans-config-api-admin-ui-plugin-openapi.yml + asyncapi/gluu-webhooks.yml
---

# Wire an Admin UI webhook

Gluu Flex's Admin UI plugin is the platform's only outbound event surface. A webhook is an HTTP call the
deployment makes when an Admin UI *feature* is exercised. Everything below is on the Config API at
`https://{host}/jans-config-api`, protected by OAuth client credentials with Admin UI scopes.

## 1. Find out what can fire

**`get-all-features`** (`GET /admin-ui/webhook/features`). The event catalogue is not a fixed published
list — it is whatever `AuiFeature` records this deployment exposes, each with an `auiFeatureId`, a
`displayName` and the `jansScope` it belongs to. Read it at runtime; do not hardcode feature ids.

## 2. Register the webhook

**`post-webhook`** (`POST /admin-ui/webhook`) with a `WebhookEntry`:

- `url`, `httpMethod`, `httpHeaders`
- `httpRequestBodyString` — a **template**, not a fixed body. Values are substituted at trigger time
  from a `shortcodeValueMap`.
- `auiFeatureIds` — the features this webhook is mapped to
- `jansEnabled` — set it false to stage a webhook without arming it

## 3. Verify the mapping

- **`get-webhooks-by-feature-id`** (`GET /admin-ui/webhook/{featureId}`) — what fires for this feature.
- **`get-features-by-webhook-id`** (`GET /admin-ui/webhook/features/{webhookId}`) — the reverse.

Check both before arming. A webhook mapped to more features than you intended fires on all of them.

## 4. Test

**`trigger-webhook`** (`POST /admin-ui/webhook/trigger/{featureId}`) fires every webhook mapped to that
feature, for real, against the live URL. There is no dry-run mode and no sandbox. Point the webhook at a
disposable endpoint first.

## What this surface does not give you

No delivery signing, no documented retry policy, no replay. None of the three is described in the
contract or the docs, so an agent must not assume any of them. Verify at the receiving end.

## Undo

**`delete-Webhook-by-inum`** (`DELETE /admin-ui/webhook/{webhookId}`) removes it; **`put-webhook`** with
`jansEnabled: false` disarms it without losing the configuration. Prefer disarming.
