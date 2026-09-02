# Channel: Paid ads (Google + Meta)

**Last verified:** 2026-08-11 · Links: [claims policy](../compliance/claims.md) · [brand voice](../brand/voice.md) · [keyword snapshots](../../data/keywords/2026-07/)

## Strategy

Google and Meta ads run against forandagainstbodycare.com and directly against Amazon listings, capitalizing on Amazon's ranking boost for external traffic. Amazon Attribution — not Google Ads' own conversion tag — is the real measurement layer, because Google's conversion tag cannot fire on amazon.com and Amazon stopped supporting GCLID capture in 2022. This means **no automated bidding strategies** — Manual CPC throughout, and Search campaign type (not Performance Max), since the plan depends on keyword-level control.

Sequencing: **Phase 1** launches direct-to-Amazon (no website hop) to get clean, fast Attribution data. **Phase 2** graduates proven themes to route through Webflow first, once Phase 1 has 2–3 weeks of data.

## Phase 1 — Direct-to-Amazon

### Campaign 1: Competitor Conquesting

Competitor names appear in keyword targeting only — **never** in headlines, descriptions, or the display URL, per the [claims policy](../compliance/claims.md) paid-ads row ("case-by-case," treated conservatively here as never-in-copy). Lead ad copy with value/category language: "Premium body care without the markup," "The accessible-luxury alternative."

| Ad group | Seed keywords (Amazon-side volume, prioritization signal only) | Negative keywords |
|---|---|---|
| Salt & Stone | salt and stone; salt and stone deodorant; salt and stone bergamot and hinoki; salt and stone santal and vetiver | perfume, candle, cologne, body mist (S&S sells fragrance/home items we don't carry) |
| Ouai | ouai; ouai shampoo and conditioner; ouai detox shampoo; ouai shampoo; ouai leave in conditioner; ouai conditioner | — |
| Necessaire | necessaire; necessaire body wash; necessaire body lotion; necessaire deodorant | — |

Ouai skews hair-care — lines up with S&V shampoo being a best seller. Necessaire has the cheapest bid floor of the three; good test candidate once conquesting has legs.

**All Amazon-side volume above is a prioritization signal, not a Google number.** Run every shortlist through Google Keyword Planner before launch — Amazon search behavior and Google search behavior diverge, sometimes sharply (see Category — Body Wash note below).

### Campaign 2: Scent & Category

| Ad group | Seed keywords | Notes |
|---|---|---|
| Hinoki & Bergamot | bergamot body wash; hinoki body wash; bergamot and hinoki; hinoki shampoo | Best-selling body wash line |
| Santal & Vetiver | vetiver; santal body wash; vetiver body wash; santal and vetiver | Best-selling body wash + shampoo line. **Skip bare "santal"** — Le Labo Santal 33 (fragrance) dominates intent. **"vetiver" needs negatives**: perfume, cologne, essential oil, candle |
| Category — Body Wash | natural body wash; all natural body wash; luxury body wash | Scent-plus-product terms (e.g. "bergamot body wash") show real Amazon volume but are thin on Google (~500/mo) — treat as long-tail additions, not headline terms |
| Category — Shampoo *(Phase 2 — hold)* | sulfate free shampoo; luxury shampoo and conditioner; luxury shampoo | Lowest-intent, highest-CPC tier — build only once Campaign 1–2 prove payback |
| Category — Lotion *(Phase 2 — hold)* | natural lotion; all natural lotion; luxury lotion | Same hold logic as above |

**Known gap:** no deodorant keyword file exists in `data/keywords/` even though H&B and S&V deodorant are best sellers — build that ad group directly in Google Keyword Planner rather than skipping deodorant.

**Amber & Sandalwood is excluded** from all campaign planning and Attribution product pools — the line is being discontinued.

### Budget allocation

- **$50/day tier (launch now):** Salt & Stone (conquesting) + Hinoki & Bergamot + Santal & Vetiver + Category — Body Wash. Covers both proven scent lines plus the strongest competitor term.
- **$100/day tier (add after week 1–2 data):** Ouai + Necessaire (conquesting) + Amber & Sandalwood scent ad group is **not** added — hold Category — Shampoo and Category — Lotion for a proven-out Phase 2 instead.

## Amazon Attribution tracking for Google Ads

A **separate** Attribution campaign, `For & Against — Google Ads`, keeps Google-driven performance isolated from the existing `For & Against Website` campaign (see the site's own Attribution structure in [amazon-attribution.md](amazon-attribution.md)).

- Each Amazon Attribution **ad group generates exactly one tag** — to get keyword-level destination routing within a single Google Ads ad group, create multiple Attribution ad groups per theme (e.g. `Salt & Stone - Storefront`, `Salt & Stone - Deodorant`, `Salt & Stone - H&B Body Wash`, `Salt & Stone - S&V Body Wash`), then apply each tag at the **keyword level** via Google Ads' keyword-level Final URL override. The ad group's default Final URL serves as the storefront fallback.
- Leave Google Ads' Campaign URL Options blank — it will conflict with the Attribution tracking parameters already embedded in the Attribution tag.
- Naming must be **identical** across Google Ads and Amazon Attribution — this is the join key for reconciliation.

### Weekly reconciliation workflow

1. Pull the Google Ads report by ad group: clicks, cost, CTR, average CPC, for the week.
2. Pull the Amazon Attribution report by matching ad group tag: attributed clicks, detail page views, add-to-cart, purchases, sales, new-to-brand %, same week.
3. Join the two by ad group name. Calculate effective CPA and ROAS yourself — neither platform computes this natively.
4. **Wait out the 14-day last-touch attribution window** before judging a cohort — a week-old cohort will look artificially weak while the window is still open.

## Phase 2 — Website-routed (once Phase 1 has data)

Route proven Phase 1 themes through existing Webflow pages (Homepage, Best Sellers, Shop by Scent), reusing the Attribution ad groups already built for those placements. This unlocks:
- A/B testing direct-to-Amazon vs. through-website conversion and ROAS
- Retargeting audiences off website visitors (not possible off Amazon click traffic)
- Room for brand storytelling before the Amazon handoff — matters more for cold, non-branded traffic than for conquesting traffic, which is already primed with comparison intent

Sequencing: launch Campaign 1–2 direct-to-Amazon first, let 2–3 weeks of Attribution data land, then decide which themes graduate to a website-routed version.

## Rules

- Comparative/dupe claims in paid ads are higher-risk than organic — see [claims policy](../compliance/claims.md). "Luxury body care without the markup" is safe; naming competitors in ad copy requires case-by-case review (treated as never, above).
- Landing pages (Phase 2): scent or product-line pages on Webflow with clear Amazon CTAs.
- Creative uses design tokens (`docs/design-system/tokens.css`) and scent accent colors; copy voice per [brand/voice.md](../brand/voice.md).
- Approved ad copy archives to `copy/ads/`; test results to `docs/learnings.md`.
- No financial/COGS figures in this file or any committed ad-copy file, per `AGENTS.md`.
