---
name: Read current and forecast electricity prices for an Amber site
description: >-
  Resolve the sites on an Amber Electric account, then read the live price and
  the forecast curve for one of them so a load can be scheduled into cheap or
  negative-price intervals.
api: openapi/amber-electric-public-api-openapi.json
base_url: https://api.amber.com.au/v1
auth: bearer
operations:
  - getSites
  - getCurrentPrices
  - getPrices
generated: '2026-07-27'
method: generated
source: openapi/amber-electric-public-api-openapi.json
---

# Read current and forecast electricity prices for an Amber site

Amber passes wholesale NEM prices through to the customer, so the price changes
every half hour and can go negative. This skill gets the live price and the
forecast curve for one site.

## Before you start

You need a bearer token. It is generated inside the logged-in Amber customer app
at <https://app.amber.com.au/developers> — open the Developers tab, switch on
Developers Mode, and click "Generate a new token". There is **no** developer
signup, sandbox, trial key or application form: only an existing Amber
electricity customer can obtain one. Send it as
`Authorization: Bearer <token>` on every call in this skill.

## Steps

1. **Resolve the site.** Call `getSites`:
   `GET /sites`
   Returns an array of sites. Take `id` — a ULID such as
   `01F5A5CRKMZ5BCX9P1S4V990AM`. It is **not** the NMI. Prefer a site whose
   `status` is `active`; `pending` sites are mid-transfer and `closed` sites are
   historical. Note the `channels[]` — `general`, `controlledLoad` and `feedIn`
   are priced separately.
2. **Get the live price.** Call `getCurrentPrices`:
   `GET /sites/{siteId}/prices/current`
   Optional: `previous=<n>` for recent settled intervals, `next=<n>` for the
   forecast curve, `resolution=30` (30 is the only accepted value here).
3. **Or get a date range.** Call `getPrices`:
   `GET /sites/{siteId}/prices?startDate=YYYY-MM-DD&endDate=YYYY-MM-DD`
   Both default to today. `resolution` accepts `5` or `30` on this operation.
4. **Interpret each element by its `type`:**
   - `ActualInterval` — settled price.
   - `CurrentInterval` — interval in progress; `estimate: true` means the price
     is not locked in yet, `false` means it is final. May carry a `range`.
   - `ForecastInterval` — AEMO-modelled; may carry a `range` of possible spot
     prices when the market is volatile.
5. **Decide on the right field.** `perKwh` is what the customer actually pays in
   cents per kWh including GST. `spotPerKwh` is the underlying NEM spot price.
   Use `perKwh` for cost decisions; use `spotPerKwh` only when reasoning about
   the market itself.
6. **Watch the risk fields.** `spikeStatus` (`none` / `potential` / `spike`) and
   `descriptor` (`extremelyLow` … `spike`) are Amber's own bands. Abort or defer
   a scheduled load when `spikeStatus` is `spike`.

## Rules

- **Never index by array position.** Return order is General > Controlled Load >
  Feed In, but the contract warns verbatim: "If a channel is added or removed
  the index offset will change. It is best to filter or group the array by
  channel type." Group by `channelType`.
- **Feed-in is a different sign of the same coin.** `feedIn` channel prices are
  what the customer is paid for export; do not mix them into consumption cost.
- **Time is NEM time (UTC+10, no DST).** `nemTime` is the *end* of the interval.
  An interval's `date` can differ from the date part of `nemTime`.
- **`descriptor: negative` is retired** — it was replaced by `extremelyLow`.
  Handle both if you parse historical data.
- **Rate limit: 50 calls per 5 minutes, per account** — not per key and not per
  site. Cache the site list; it changes almost never. Poll prices at most once
  per interval boundary plus a short settle delay.
- **Retry safely.** All three operations are GETs, so retries are safe — but
  each retry spends quota. Back off on 429 (undeclared in the contract but real)
  and on 500.
- 401 means the token is missing or invalid: `{"message":"Unauthorized"}`.
  404 on a site-scoped call means the siteId is wrong — re-run `getSites`.

## Related

- skills/amber-electric-usage-history.md
- conventions/amber-electric-conventions.yml
- data-model/amber-electric-data-model.yml
- rate-limits/amber-electric-rate-limits.yml
