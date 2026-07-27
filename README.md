# Amber Electric (amber-electric)

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
