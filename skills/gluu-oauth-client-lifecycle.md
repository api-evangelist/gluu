---
name: gluu-oauth-client-lifecycle
description: Register an OAuth client dynamically on a Janssen Auth Server, run an authorization code flow with PKCE, and revoke cleanly.
api: gluu:jans-auth-server-api
spec: openapi/gluu-jans-auth-server-openapi.yml
operations:
  - post-register
  - get-register
  - put-register
  - delete-register
  - get_authorize
  - post_par
  - post-token
  - get-userinfo
  - post-introspection
  - revoke
  - global-token-revocation
  - end_session
generated: '2026-09-12'
method: generated
source: openapi/gluu-jans-auth-server-openapi.yml + conventions/gluu-conventions.yml
---

# Register a client and run an authorization code flow

The Janssen Auth Server is a certified OpenID Provider running at `https://{host}/jans-auth`. Start from
discovery, never from a hardcoded path.

## 1. Discover

Fetch `/.well-known/openid-configuration` on the deployment host. Read `registration_endpoint`,
`authorization_endpoint`, `token_endpoint`, `userinfo_endpoint`, `revocation_endpoint` and
`pushed_authorization_request_endpoint` from it.

## 2. Register a client

**`post-register`** (`POST /restv1/register`). The response returns `client_id`, optionally
`client_secret`, plus **`registration_client_uri`** and **`registration_access_token`**.

Store those last two. They are the only way to later read (`get-register`), update (`put-register`) or
delete (`delete-register`) the client — and `delete-register` is your undo for this step.

## 3. Authorize

Build the request with PKCE: `code_challenge_method=S256`, a `code_challenge`, a `state`, and a `nonce`
of at least 16 characters. Send the user to **`get_authorize`** (`GET /restv1/authorize`).

For anything sensitive, push the request first with **`post_par`** (`POST /restv1/par`, RFC 9126) and
send the returned `request_uri` to the authorization endpoint instead of query parameters.

## 4. Exchange

**`post-token`** (`POST /restv1/token`) with `grant_type=authorization_code`, the `code`, the
`code_verifier` and the same `redirect_uri`. Then **`get-userinfo`** with the access token for claims.

## 5. Verify a token you were handed

**`post-introspection`** (`POST /restv1/introspection`, RFC 7662) tells you whether a token is active and
what scopes it carries, without acting on it. This is the safe read-only check before any privileged
call.

## 6. Undo

- **`revoke`** (`POST /restv1/revoke`, RFC 7009) revokes a refresh token. Already-issued access tokens
  stay valid until they expire unless the resource server introspects them — Gluu publishes no window
  for this, so do not assume immediate global effect.
- **`global-token-revocation`** revokes across the subject.
- **`end_session`** ends the OP session.
- **`delete-register`** removes the client entirely.

## Errors

The Auth Server returns the OAuth 2.0 error object — `error`, `error_description`, `details` — per
RFC 6749 §5.2. Branch on `error`. `401` means the client credentials or token are wrong; `400` means the
request is malformed (a mismatched `redirect_uri` and a bad `code_verifier` both land here).

## Retry discipline

No endpoint accepts an idempotency key. A retried `post-register` creates a **second** client. On a
timeout, call `get-register` with the registration access token (if you kept it) or search before
re-registering.
