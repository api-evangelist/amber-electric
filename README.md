# Amber Electric (amber-electric)

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

Amber Electric is an Australian electricity retailer (ABN 98623603805) that sells wholesale National Electricity Market pricing straight through to residential customers on a flat monthly membership, rather than marking energy up, and automates home batteries, solar exports and EV charging against those half-hourly prices. It sits at the retail end of the Australian energy value chain, between AEMO's wholesale market and the household meter. Its API posture is unusually honest and unusually split. Amber publishes a real, verbatim OpenAPI 3.0.0 contract for a REST API at `https://api.amber.com.au/v1` covering sites, prices, forecasts and usage, but the token that unlocks it is generated inside the logged-in customer app at https://app.amber.com.au/developers, so the API is customer-account-required rather than self-serve — a developer who is not an Amber customer cannot obtain a key. One endpoint is the exception: the spec explicitly declares `security: []` on `GET /state/{state}/renewables/current`, and that grid renewables-percentage feed really does answer anonymously for NSW, VIC, QLD and SA, so open market data and gated consumer data live inside the same contract. Separately, Amber is a designated Consumer Data Right energy data holder that is genuinely live, not merely designated: it is listed on the ACCC CDR Register with a working public base URI at `https://public.cdr.amber.com.au`, whose CDS discovery endpoints and anonymously-served OpenID Connect configuration advertise the full Consumer Data Standards energy scope set behind `private_key_jwt` and CDR accreditation.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/amber-electric/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/amber-electric/refs/heads/main/apis.yml)

## Tags

- Energy
- Australia
- Electricity
- Utilities
- Consumer Data Right
- Energy Markets
- Renewables
- Solar
- Batteries
- DER
- Smart Metering
- Wholesale Pricing

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

### Amber Electric Public API

Amber Electric's own documented REST API, described by a verbatim OpenAPI 3.0.0 contract the company publishes in its public GitHub repository. Five read-only operations: list the sites on your account, fetch actual, current and forecast half-hourly prices for a site, fetch usage for a site, and read the current renewables percentage for a National Electricity Market state. Authentication is HTTP bearer (declared in the spec as the `apiKey` security scheme, type http / scheme bearer) and the token is generated from the Developers tab inside the logged-in Amber customer app, so four of the five operations require an active Amber account. The renewables operation carries an explicit `security: []` override and was confirmed to return 200 anonymously on 2026-07-27.

- **Human URL:** [https://app.amber.com.au/developers](https://app.amber.com.au/developers)
- **Base URL:** `https://api.amber.com.au/v1`

#### Tags

- Energy
- Electricity
- Pricing
- Usage
- Renewables
- Australia

#### Properties

- [OpenAPI](openapi/amber-electric-public-api-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://app.amber.com.au/developers)
- [API Reference](https://app.amber.com.au/developers/documentation)
- [Source Code](https://github.com/amberelectric/public-api)
- [Support](https://github.com/amberelectric/public-api/discussions)

### Amber Electric Consumer Data Right Energy API

Amber's Consumer Data Right energy data-holder surface, mandated by the Australian CDR regime extended from banking into energy and administered by the ACCC with standards set by the Data Standards Body. Amber is listed on the public CDR Register as an energy data holder brand (dataHolderBrandId `54968899-b7b5-ef11-95f6-6045bd3f1493`, ABN 98623603805, lastUpdated 2026-07-23) with the public base URI `https://public.cdr.amber.com.au`. The implementation was verified live on 2026-07-27, not merely claimed: the Consumer Data Standards discovery endpoints `/cds-au/v1/discovery/status` and `/cds-au/v1/discovery/outages` both returned 200 with conformant CDS payloads, and `/.well-known/openid-configuration` is served anonymously. Access is accredited-only — dynamic client registration at `https://secure.cdr.amber.com.au/connect/register` with `private_key_jwt` client authentication and PAR, reachable only by an accredited CDR data recipient holding a software statement assertion from the CDR Register. No general developer can call it.

- **Human URL:** [https://www.cdr.gov.au/](https://www.cdr.gov.au/)
- **Base URL:** `https://public.cdr.amber.com.au/cds-au/v1`

#### Tags

- Consumer Data Right
- Energy
- Open Data
- Regulated
- OpenID Connect
- Australia

#### Properties

- [Authentication](authentication/amber-electric-cdr-openid-configuration.json)
- [OpenID Connect Discovery](https://public.cdr.amber.com.au/.well-known/openid-configuration)
- [Documentation](https://consumerdatastandardsaustralia.github.io/standards/)
- [Registry](https://api.cdr.gov.au/cdr-register/v1/energy/data-holders/brands/summary)
- [Documentation](https://www.cdr.gov.au/consumers/energy)

## Common Properties

- [Website](https://amber.com.au/)
- [Documentation](https://app.amber.com.au/developers)
- [GitHub Organization](https://github.com/amberelectric)
- [Blog](https://amber.com.au/blog)
- [Support](https://help.amber.com.au/)
- [Privacy](https://amber.com.au/privacy)
- [Sign Up](https://app.amber.com.au/developers)

## Maintainers

- **Kin Lane** — kin@apievangelist.com
