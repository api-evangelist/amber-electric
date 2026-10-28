# Amber Electric

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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Amber Electric is an Australian electricity retailer (ABN 98623603805) that sells wholesale National Electricity Market pricing straight through to residential customers on a flat monthly membership, rather than marking energy up, and automates home batteries, solar exports and EV charging against those half-hourly prices. It sits at the retail end of the Australian energy value chain, between AEMO's wholesale market and the household meter. Its API posture is unusually honest and unusually split. Amber publishes a real, verbatim OpenAPI 3.0.0 contract for a REST API at https://api.amber.com.au/v1 covering sites, prices, forecasts and usage, but the token that unlocks it is generated inside the logged-in customer app at https://app.amber.com.au/developers, so the API is customer-account-required rather than self-serve — a developer who is not an Amber customer cannot obtain a key. One endpoint is the exception: the spec explicitly declares `security: []` on GET /state/{state}/renewables/current, and that grid renewables-percentage feed really does answer anonymously for NSW, VIC, QLD and SA, so open market data and gated consumer data live inside the same contract. Separately, Amber is a designated Consumer Data Right energy data holder that is genuinely live, not merely designated: it is listed on the ACCC CDR Register with a working public base URI at https://public.cdr.amber.com.au, whose CDS discovery endpoints and anonymously-served OpenID Connect configuration advertise the full Consumer Data Standards energy scope set behind private_key_jwt and CDR accreditation.

**Areas:** Energy · Australia · Electricity · Utilities · Consumer Data Right · Energy Markets · Renewables · Solar · Batteries · DER · Smart Metering · Wholesale Pricing

## Amber Electric on APIs.io

Everything profiled for Amber Electric is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/amber-electric/](https://apis.io/providers/amber-electric/)** — or go straight to one of the APIs or properties below.

### APIs (2)

- [Amber Electric Consumer Data Right Energy API](https://apis.io/apis/amber-electric/amber-electric-cdr-energy-api/) — Amber's Consumer Data Right energy data-holder surface, mandated by the Australian CDR regime extended from banking into energy and administered by the ACCC…
- [Amber Electric Amber API](https://apis.io/apis/amber-electric/amber-electric-amber-api/) — The Amber API from Amber Electric — 5 operation(s) for amber.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [amber-electric](https://apis.io/servers/amber-electric/amber-electric/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Amber Electric Agentic Access](https://apis.io/agentic-access/amber-electric/amber-electric-agentic-access/)

#### JSON Schema (8)

The data models the API reads and writes.

- [ActualInterval](https://apis.io/schemas/amber-electric/amber-electric-actual-interval/)
- [ActualRenewable](https://apis.io/schemas/amber-electric/amber-electric-actual-renewable/)
- [CurrentInterval](https://apis.io/schemas/amber-electric/amber-electric-current-interval/)
- [CurrentRenewable](https://apis.io/schemas/amber-electric/amber-electric-current-renewable/)
- [ForecastInterval](https://apis.io/schemas/amber-electric/amber-electric-forecast-interval/)
- [ForecastRenewable](https://apis.io/schemas/amber-electric/amber-electric-forecast-renewable/)
- [Site](https://apis.io/schemas/amber-electric/amber-electric-site/)
- [Usage](https://apis.io/schemas/amber-electric/amber-electric-usage/)

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Amber Electric Context](https://apis.io/jsonld/amber-electric/amber-electric-context/)

#### Collections (1)

Postman and Open Collection request collections.

- [Amber Electric Public API](https://apis.io/collections/amber-electric/open-amber-electric-public-api/)

#### Overlays (3)

OpenAPI overlays, including conversational phrasing for each operation.

- [Amber Electric Public Api Overlay](https://apis.io/overlays/amber-electric/amber-electric-public-api-overlay/)
- [Amber Electric Sites Api Overlay](https://apis.io/overlays/amber-electric/amber-electric-sites-api-overlay/)
- [Amber Electric State Api Overlay](https://apis.io/overlays/amber-electric/amber-electric-state-api-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [Amber Electric API Rules](https://apis.io/rules/amber-electric/amber-electric-rules/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Amber Electric Authentication](https://apis.io/security/amber-electric/amber-electric-authentication/)
- [Amber Electric Domain Security](https://apis.io/security/amber-electric/amber-electric-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Amber Electric Scopes](https://apis.io/scopes/amber-electric/amber-electric-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Amber Electric Plans Pricing](https://apis.io/plans/amber-electric/amber-electric-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Amber Electric Rate Limits](https://apis.io/rate-limits/amber-electric/amber-electric-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.amber.com.au/)
- [Documentation](https://app.amber.com.au/developers/documentation/)
- [Getting started](https://help.amber.com.au/hc/en-us/articles/360037454111-What-is-the-onboarding-process)
- [API reference](https://app.amber.com.au/developers/documentation)
- [Pricing](https://www.amber.com.au/pricing)
- [Sign up](https://app.amber.com.au/signup)
- [Support](https://help.amber.com.au/hc/en-us)
- [Blog](https://www.amber.com.au/blog)
- [Terms of service](https://www.amber.com.au/terms-and-conditions/terms)
- [Privacy policy](https://www.amber.com.au/legal/privacy)
- [GitHub](https://github.com/amberelectric)
- [Status](https://www.amber.com.au/status)

## API Evangelist rating

**Kin Score 64.2** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 68.0 |
| Contract governance | 35.6 |
| Operational transparency | 39.5 |
| Developer ergonomics | 66.1 |
| Access clarity | 65.8 |

**Agent readiness 44.7** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | na |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | na |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | partial |
| Rate Limit Signal | verified |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | na |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 45.1** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/amber-electric/](https://apis.io/providers/amber-electric/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Amber Electric: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/amber-electric/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
