---
name: Pull interval usage and cost history for an Amber site
description: >-
  Walk an Amber Electric account's sites and download half-hourly consumption,
  generation, cost and data quality for a date range, in 90-day windows.
api: openapi/amber-electric-public-api-openapi.json
base_url: https://api.amber.com.au/v1
auth: bearer
operations:
  - getSites
  - getUsage
generated: '2026-07-27'
method: generated
source: openapi/amber-electric-public-api-openapi.json
---

# Pull interval usage and cost history for an Amber site

Use this to reconcile a bill, chart consumption against price, or measure what a
battery or solar system actually did.

## Before you start

A bearer token generated from the Developers tab of
<https://app.amber.com.au/developers> is required — see
`skills/amber-electric-site-prices.md` for how it is minted. Send it as
`Authorization: Bearer <token>`.

## Steps

1. **Resolve the site.** `GET /sites` (`getSites`). Take the `id` of the site you
   want and note its `channels[]` — each channel is billed separately.
2. **Request a window.** Call `getUsage`:
   `GET /sites/{siteId}/usage?startDate=YYYY-MM-DD&endDate=YYYY-MM-DD`
   Both parameters are **required** here (unlike `getPrices`, where they
   default to today). `resolution` accepts only `30`.
3. **Chunk long ranges.** The operation description states plainly: "The API can
   only return 90-days worth of data." Split anything longer into consecutive
   ≤90-day windows and concatenate.
4. **Read each record.** A `Usage` record extends the price `Interval`, so it
   carries the full price context as well as:
   - `channelIdentifier` — matches `Channel.identifier` from step 1 (e.g. `E1`).
   - `kwh` — energy for the interval. **Generation and export are negative.**
   - `cost` — cents for the interval, GST inclusive. Negative on export credit.
   - `quality` — `billable` or `estimated`. Estimated readings mean the meter
     could not be contacted; they are not what will appear on the bill.
5. **Aggregate by channel, then by day.** Group on `channelType` /
   `channelIdentifier` first — never on array position.

## Rules

- **Filter out or flag `quality: estimated`** before presenting a figure as
  billed cost. Mixing estimated and billable intervals silently produces a
  number that will not match the customer's invoice.
- **Signs matter.** Negative `kwh` is export/generation, not an error. Summing
  raw `kwh` across a general and a feed-in channel nets consumption against
  export and is almost never what you want.
- **Time is NEM time (UTC+10, no DST).** `nemTime` is the end of the interval;
  `date` is the trading date and can differ from the date part of `nemTime`
  because the trading day's last interval ends at 12:00 the next day. Day
  boundaries computed in local browser time will be off.
- **Usage lags.** Meter data arrives from the metering company; recent intervals
  may be missing or estimated and later restated.
- **Rate limit: 50 calls per 5 minutes, per account.** A multi-year backfill in
  90-day windows is dozens of calls — pace it, and cache what you already have.
- Errors are `{"message": "..."}`. 400 usually means a malformed date, a window
  over 90 days, or a missing startDate/endDate. 401 is a bad token. 404 is a bad
  siteId.

## Related

- skills/amber-electric-site-prices.md
- data-model/amber-electric-data-model.yml
- errors/amber-electric-problem-types.yml
