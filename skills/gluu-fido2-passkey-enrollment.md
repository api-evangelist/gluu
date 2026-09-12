---
name: gluu-fido2-passkey-enrollment
description: Enroll and authenticate a passkey against the Janssen FIDO2 server, and inspect enrolled devices over SCIM.
api: gluu:jans-fido2-api
spec: openapi/gluu-jans-fido2-openapi.yml
operations:
  - get-fido2-configuration
  - attestation-options
  - attestation-result
  - options
  - result
  - get-fido2-devices
  - get-fido2-device-by-id
  - delete-fido2-device-by-id
generated: '2026-09-12'
method: generated
source: openapi/gluu-jans-fido2-openapi.yml + openapi/gluu-jans-scim-openapi.yml
---

# Enroll and use a passkey

The FIDO2 server runs at `https://{host}/jans-fido2` and implements the W3C WebAuthn ceremonies in two
halves — the server produces options, the authenticator produces a response, the server verifies it.

## Discover

**`get-fido2-configuration`** (`GET /jans-fido2/restv1/configuration`) returns the endpoints and the
policy this deployment enforces. Read it before building a ceremony; attestation policy in particular is
deployment-specific.

## Register a credential (attestation)

1. **`attestation-options`** (`POST /restv1/attestation/options`) — send the username and display name.
   You get back the `PublicKeyCredentialCreationOptions`: challenge, rp, user, pubKeyCredParams,
   authenticatorSelection.
2. Hand those to `navigator.credentials.create()` in the browser. **This half is not an API call** — no
   agent can complete it without a real authenticator and a present human.
3. **`attestation-result`** (`POST /restv1/attestation/result`) — post the authenticator's response back
   for verification and storage.

## Authenticate (assertion)

1. **`options`** (`POST /restv1/assertion/options`) — get the `PublicKeyCredentialRequestOptions`.
2. `navigator.credentials.get()` in the browser.
3. **`result`** (`POST /restv1/assertion/result`) — post the assertion for verification.

## Manage enrolled devices

Devices are SCIM resources, not FIDO2 endpoints — cross to the SCIM API
(`openapi/gluu-jans-scim-openapi.yml`):

- **`get-fido2-devices`** (`GET /Fido2Devices`), filtered on `userId`, lists a person's enrollments.
- **`get-fido2-device-by-id`** reads one; `status` is one of `registered`, `pending`, `compromised`,
  `canceled`.
- **`delete-fido2-device-by-id`** removes an enrollment permanently — there is no restore. Never remove
  a user's last credential without confirming they have another factor.

## Operational signal

The server exposes real metrics endpoints — adoption, attestation rejections, errors, performance,
device breakdown (`/restv1/metrics/analytics/*`) and MDS trust health
(`/restv1/trust/mds/health`). Use `attestation-rejections` when enrollments start failing: an expired
or undownloadable FIDO Metadata Service blob is a known failure mode this deployment reports there.
