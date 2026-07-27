---
name: Read live grid renewables for an Australian state
description: >-
  Fetch the current, recent and forecast renewable-energy percentage of the
  National Electricity Market grid for NSW, VIC, QLD or SA using Amber
  Electric's one anonymous endpoint — no account, no key, no signup.
api: openapi/amber-electric-public-api-openapi.json
base_url: https://api.amber.com.au/v1
auth: none
operations:
  - getCurrentRenewables
generated: '2026-07-27'
method: generated
source: openapi/amber-electric-public-api-openapi.json
---

# Read live grid renewables for an Australian state

This is the only Amber Electric operation that answers without a credential. It
is the right tool for "how green is the grid right now in Victoria" and for
scheduling a load against greener intervals.

## When to use it

- Deciding whether now is a good time to run a heavy appliance, charge an EV or
  run a pool pump on grid greenness rather than on price.
- Building a grid-carbon signal into a dashboard or automation.
- Any case where you are not an Amber customer — this endpoint needs nothing.

## Steps

1. Choose a National Electricity Market state. Valid values are exactly
   `nsw`, `sa`, `qld`, `vic` (lowercase). Tasmania and WA are not served.
2. Call `getCurrentRenewables`:
   `GET /state/{state}/renewables/current`
   Send no `Authorization` header. The operation declares `security: []`.
3. Optional windowing:
   - `previous=<n>` — also return the previous n settled intervals.
   - `next=<n>` — also return the next n forecast intervals.
   - `resolution=5|30` — interval length in minutes, default 30.
4. Read the array. Each element carries a `type` discriminator — switch on it:
   - `ActualRenewable` — a settled interval.
   - `CurrentRenewable` — the interval in progress.
   - `ForecastRenewable` — AEMO-modelled future interval.
5. Use `renewables` (percentage, 0-100) for the number and `descriptor`
   (`best`, `great`, `ok`, `notGreat`, `worst`) for a human-readable band.

## Example

```
GET https://api.amber.com.au/v1/state/vic/renewables/current?next=1&previous=1
```

```json
[
  {"type":"ActualRenewable","duration":30,"date":"2026-07-28","startTime":"2026-07-27T20:00:01Z","endTime":"2026-07-27T20:30:00Z","nemTime":"2026-07-28T06:30:00+10:00","renewables":56.882,"descriptor":"best"},
  {"type":"CurrentRenewable","duration":30,"date":"2026-07-28","startTime":"2026-07-27T20:30:01Z","endTime":"2026-07-27T21:00:00Z","nemTime":"2026-07-28T07:00:00+10:00","renewables":55.266999999999996,"descriptor":"best"},
  {"type":"ForecastRenewable","duration":30,"date":"2026-07-28","startTime":"2026-07-27T21:00:01Z","endTime":"2026-07-27T21:30:00Z","nemTime":"2026-07-28T07:30:00+10:00","renewables":53.047999999999995,"descriptor":"best"}
]
```

Captured live on 2026-07-27. States diverge sharply at the same instant — NSW
read 7.4% while Victoria read 55.3%.

## Rules

- **Time is NEM time.** `nemTime` is the time at the *end* of the interval in
  UTC+10 with no daylight saving. `startTime` and `endTime` are UTC. The `date`
  field is the trading date and can differ from the date part of `nemTime`,
  because the last interval of a trading day ends at 12:00 the following day.
- **Pace your calls.** The account limit is 50 calls per 5 minutes. Intervals
  are 30 minutes long by default, so polling more than once or twice a minute
  buys nothing.
- **Handle 404** — an unrecognised state returns 404, not an empty array.
- **Handle 429** — undeclared in the contract but returned in practice when the
  rate limit is exceeded. Back off; do not hot-loop.
- Errors are `{"message": "..."}`, not RFC 9457 problem+json.

## Related

- conventions/amber-electric-conventions.yml
- errors/amber-electric-problem-types.yml
- rate-limits/amber-electric-rate-limits.yml
