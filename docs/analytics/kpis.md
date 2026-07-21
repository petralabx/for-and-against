# KPI definitions

**Last verified:** 2026-07-14 — definitions locked here so every report means the same thing.

Raw analytics data does NOT live in this repo (GA4 → BigQuery native export; Amazon SP-API reports → same store or Sheets). This repo holds definitions, report config, and small auto-generated digests.

| KPI | Definition |
|---|---|
| Units / Revenue | Amazon ordered units / ordered product sales, per SKU per week |
| Conversion rate | Amazon unit session percentage (units ÷ sessions) |
| TACoS | Total ad spend ÷ total revenue (all channels' spend vs Amazon revenue) |
| ACoS | Amazon ad spend ÷ Amazon ad-attributed sales |
| External-traffic share | Sessions from Amazon Attribution tags ÷ total sessions |
| Refill attach rate | 2L pouch units ÷ matching 500ml units, trailing 90d |
| Organic rank | Position for tracked keywords (from data/keywords snapshots) |

Weekly digest (auto-generated once wired): `docs/analytics/weekly-digest.md` — sales by SKU, WoW deltas, TACoS, top search terms, price-watch alerts.
