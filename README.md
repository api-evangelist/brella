# Brella (brella)

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

Brella is an event networking and engagement platform for conferences, trade shows, livestreams, and hybrid events, best known for AI-powered attendee matchmaking and one-to-one meeting booking. Organizers run events, attendees, speakers, sponsors, schedules, and networking through Brella's web and mobile apps.

## Access model: public REST API, but gated

**Yes — Brella exposes a documented public REST API.** It is the Brella Integration API, documented at [developer.brella.io](https://developer.brella.io/), with base URL `https://api.brella.io/api/integration`.

Access is **gated**, not open self-service:

- The API key feature is activated on an organization's account only after the integration is enabled by Brella (a paid engagement).
- An **organization admin** generates an organization-level API key from the Brella admin panel (Organization / API key). The key is shown only once at creation.
- Authenticate by sending the key in the **`Brella-Api-Access-Token`** request header. Every request must also send `Content-Type: application/json` and `Accept: application/vnd.brella.v4+json` (the Accept header selects the API version, **v4**), over SSL/TLS.

The API is **read-oriented** — it lets you pull event data out of Brella (events, attendees, speakers, sponsors, timeslots, invites) to sync into registration, CRM, and analytics tools. Brella also documents **outbound webhooks** for change notifications. There is **no documented WebSocket or SSE (realtime streaming) API**.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/brella/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/brella/refs/heads/main/apis.yml)

## Tags

- Events
- Event Networking
- Matchmaking
- Event Platform
- Attendees
- Engagement
- Conferences
- SaaS

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## Base URL and authentication

- **Base URL:** `https://api.brella.io/api/integration`
- **Auth header:** `Brella-Api-Access-Token: <your-api-key>`
- **Version header:** `Accept: application/vnd.brella.v4+json`
- **Pagination:** mandatory on all list endpoints — `page[size]` (max 500) and `page[number]`
- **Rate limits:** no hard cap published; Brella recommends no more than ~10 requests/second and reserves the right to block unreasonable volume

## APIs

### Brella Events API

List the events belonging to a Brella organization, with mandatory pagination and optional ISO 8601 date-range filters.

- **Human URL:** [https://developer.brella.io/list-all-events](https://developer.brella.io/list-all-events)
- **Base URL:** `https://api.brella.io/api/integration`
- `GET /organizations/{organizationId}/events`

### Brella Attendees API

List attendees for a specific event, with `created_from` / `created_to` / `updated_from` / `updated_to` filters for incremental syncs.

- **Human URL:** [https://developer.brella.io/list-all-attendees](https://developer.brella.io/list-all-attendees)
- `GET /organizations/{organizationId}/events/{eventId}/attendees`

### Brella Speakers API

List the speakers configured for a specific event.

- **Human URL:** [https://developer.brella.io/list-all-speakers](https://developer.brella.io/list-all-speakers)
- `GET /organizations/{organizationId}/events/{eventId}/speakers`

### Brella Sponsors API

List sponsors and exhibitors for a specific event.

- **Human URL:** [https://developer.brella.io/list-all-sponsors](https://developer.brella.io/list-all-sponsors)
- `GET /organizations/{organizationId}/events/{eventId}/sponsors`

### Brella Schedule API

List all timeslots (both networking and content types) for a specific event — titles, subtitles, tracks, locations, tags, and formatted content.

- **Human URL:** [https://developer.brella.io/list-all-timeslots](https://developer.brella.io/list-all-timeslots)
- `GET /organizations/{organizationId}/events/{eventId}/timeslots`

### Brella Invites API

List the invites issued for a specific event.

- **Human URL:** [https://developer.brella.io/list-all-invites](https://developer.brella.io/list-all-invites)
- `GET /organizations/{organizationId}/events/{eventId}/invites`

### Brella Webhooks

Outbound HTTP POST callbacks fired when invites, attendees, speakers, sponsors, ticket purchases, or ticket types are created, updated, or deleted. Server-to-endpoint HTTP — not a WebSocket or realtime push transport.

- **Human URL:** [https://developer.brella.io/webhooks](https://developer.brella.io/webhooks)

## Modeled but not path-confirmed

The developer portal also documents an **RSVP & session check-in** surface and a **Statistics & analytics** surface (meetings, chats, tracking sessions). Their exact paths were not confirmed at authoring time and are intentionally **not** modeled in the OpenAPI. See `review.yml`.

## Artifacts

- [OpenAPI](openapi/brella-openapi.yml)
- [Postman Collection](collections/brella.postman_collection.json)
- [Open Collection](collections/brella.opencollection.json)
- [Authentication](authentication/brella-authentication.yml)
- [Plans / Pricing](plans/brella-plans-pricing.yml)
- [Rate Limits](rate-limits/brella-rate-limits.yml)
- [FinOps](finops/brella-finops.yml)
- [Domain Security](security/brella-domain-security.yml)
- [Review](review.yml)

## Common Properties

- [Domain Security](security/brella-domain-security.yml)
- [Authentication](authentication/brella-authentication.yml)
- [GitHub Organization](https://github.com/brella)
- [LinkedIn](https://www.linkedin.com/company/brella)
- [Website](https://www.brella.io)
- [Documentation](https://developer.brella.io/)
- [Plans](plans/brella-plans-pricing.yml)
- [Rate Limits](rate-limits/brella-rate-limits.yml)
- [Fin Ops](finops/brella-finops.yml)
- [Blog](https://www.brella.io/blog)

## Pricing

Brella is a **contact-sales SaaS** platform priced per event / license; it does not publish public self-service pricing tiers, and the Integration API is not separately metered. See `plans/brella-plans-pricing.yml` (reconciled: false).

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
