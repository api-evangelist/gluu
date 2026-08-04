# Gluu (gluu)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Gluu is a technology company that specializes in providing identity and access management solutions for businesses. Their platform allows organizations to centrally manage the authentication and authorization of users across various applications and systems, ensuring secure access to sensitive data and resources.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/gluu/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/gluu/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- Access Management
- Authentication
- Authorization
- IAM
- Identities
- OAuth
- OpenID Connect

## Timestamps

- **Created:** 2025-08-14
- **Modified:** 2026-04-28

## APIs

### Gluu Flex

Gluu Flex is the commercial, self-hosted enterprise distribution of the Linux Foundation Janssen Project. It provides a cloud-native digital identity platform with OAuth 2.0, OpenID Connect, FIDO, SCIM, and UMA capabilities, deployable via Helm charts with auto-scaling support.

- **Human URL:** [https://gluu.org/flex/](https://gluu.org/flex/)
- **Base URL:** `https://docs.gluu.org/`

#### Tags

- Authentication
- Authorization
- IAM
- OAuth
- OpenID Connect

#### Properties

- [Documentation](https://docs.gluu.org/)
- [Getting Started](https://docs.gluu.org/head/admin-guide/quick-start/)
- [GitHub Repository](https://github.com/GluuFederation/flex)
- [Pricing](https://gluu.org/pricing/)
- [Postman Collection](collections/gluu.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/gluu.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Janssen Project

The Janssen Project is the upstream Linux Foundation open-source identity platform that powers Gluu Flex. It implements OAuth 2.0, OpenID Connect, FIDO 2.0, SCIM, UMA, and CIBA, providing a federated identity provider, authorization server, and FIDO server.

- **Human URL:** [https://jans.io/](https://jans.io/)
- **Base URL:** `https://docs.jans.io/`

#### Tags

- Authentication
- Authorization
- Linux Foundation
- OAuth
- Open Source
- OpenID Connect

#### Properties

- [Documentation](https://docs.jans.io/)
- [GitHub Repository](https://github.com/JanssenProject/jans)
- [Reference](https://docs.jans.io/head/admin/reference/)
- [Postman Collection](collections/gluu.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/gluu.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Cedarling

Cedarling is an embeddable Policy Decision Point (PDP) built in Rust that runs anywhere and returns authorization decisions in under 50 microseconds based on declarative Cedar access policies. It validates JWT tokens and applies policies to deliver fine-grained authorization.

- **Human URL:** [https://gluu.org/cedarling/](https://gluu.org/cedarling/)

#### Tags

- Authorization
- Cedar
- PDP
- Policy
- Rust

#### Properties

- [Documentation](https://docs.jans.io/head/cedarling/cedarling-overview/)
- [GitHub Repository](https://github.com/JanssenProject/jans/tree/main/jans-cedarling)
- [Postman Collection](collections/gluu.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/gluu.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Agama Lab

Agama Lab is a developer portal for authoring Cedar schema and policies, building authentication workflows using the Agama domain specific language, and managing hosted Gluu infrastructure.

- **Human URL:** [https://gluu.org/agama-lab/](https://gluu.org/agama-lab/)

#### Tags

- Authentication
- Developer Portal
- DSL
- Workflows

#### Properties

- [Documentation](https://docs.gluu.org/agama-lab/)
- [Portal](https://cloud.gluu.org/agama-lab)
- [Postman Collection](collections/gluu.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/gluu.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [LinkedIn](https://www.linkedin.com/company/gluufederation)
- [Website](https://gluu.org/)
- [Documentation](https://docs.gluu.org/)
- [Blog](https://gluu.org/blog/)
- [Support](https://help.gluu.org/)
- [GitHub Organization](https://github.com/GluuFederation)
- [Pricing](https://gluu.org/pricing/)
- [Contact](https://gluu.org/contact/)
- [Community](https://gluu.org/community/)
- [Integrations](https://gluu.org/partners/)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
