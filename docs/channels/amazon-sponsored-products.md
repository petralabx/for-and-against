# Channel: Amazon Sponsored Products

**Last verified:** 2026-10-21 · Links: [paid ads (Google/Meta)](paid-ads.md) · [Amazon Attribution](amazon-attribution.md) · [keyword snapshots](../../data/keywords/2026-07/)

Campaign structure for the October 2026 rebuild of the For & Against Sponsored Products account, built as Amazon Bulksheets 2.0 uploads. Break-even and target ACoS figures are derived from COGS and are deliberately kept out of this repo per `AGENTS.md`.

## Why the rebuild

Six months of data (Apr 13 – Oct 2026) showed the same pattern every review: scent-specific exact keywords were the only consistent winners, while broad, generic (`body wash`, `luxury body wash`, `native body wash`), auto and competitor-ASIN targeting lost money. The single biggest loss was structural: the old Scent Keywords ad group held three products, so when Santal & Vetiver body wash (FNA306-500) went out of stock in late August, santal/vetiver clicks kept landing on the Amber or Bergamot bottle.

## Structure rules

1. **One SKU per ad group.** Every keyword lands on the bottle the shopper searched for. Only exception: the branded-keyword ad group in Brand Defense.
2. **Scent exact keywords get their own campaign and budget**, separate from generic and discovery traffic.
3. **Discovery = auto, close match only** (loose match, substitutes, complements paused), low bids, ~$10/day. Its job is finding search terms to promote to exact.
4. **Competitor names only in long-tail scent terms** (e.g. `salt and stone santal vetiver body wash`). Never competitor head terms or competitor ASIN targeting.
5. **No generic head terms** (`body wash`, `body lotion for women`, `hand soap`).
6. **Cross-negatives:** every exact keyword is a negative exact in its product line's Discovery campaign.
7. Naming: `FNA | SP | [Product] | [Scent] | [Exact/Discovery]`. Bidding: Dynamic bids – down only; exact scent campaigns get +20% top of search.

## Campaigns

| Campaign | Ad group → SKU | Start | Budget/day | State |
|---|---|---|---|---|
| FNA \| SP \| Body Wash \| B&H \| Exact | B&H Body Wash 500ml → FNA305-500 | Oct 26 | $25 | enabled |
| FNA \| SP \| Body Wash \| S&V \| Exact | S&V Body Wash 500ml → FNA306-500 | Oct 26 | $25 | **paused** until FNA306-500 restock is checked in |
| FNA \| SP \| Body Wash \| Men's \| Exact | Men's → FNA305-500 (swap to FNA306-500 after restock) | Oct 26 | $10 | enabled |
| FNA \| SP \| Body Wash \| Discovery (auto) | Auto B&H → FNA305-500; Auto S&V → FNA306-500 (paused) | Oct 26 | $10 | enabled |
| FNA \| SP \| Brand Defense | See below | Oct 26 | $8 | enabled |
| FNA \| SP \| Hand Wash \| Exact | B&H → FNA505-500; S&V → FNA506-500 | Oct 26 | $10 | enabled |
| FNA \| SP \| Lotion \| B&H \| Exact | B&H Lotion 500ml → FNA605-500 | Nov 1 | $15 | enabled (uploaded) |
| FNA \| SP \| Lotion \| S&V \| Exact | S&V Lotion 500ml → FNA606-500 | Nov 1 | $15 | uploaded, **no ad** — FNA606-500 ineligible |
| FNA \| SP \| Lotion \| Discovery (auto) | Auto B&H → FNA605-500; Auto S&V → FNA606-500 (no ad) | Nov 1 | $10 | enabled (uploaded) |
| FNA \| SP \| Deodorant \| B&H \| Exact | B&H Deodorant → FNA755-75 | Nov 1 | $10 | enabled (uploaded) |
| FNA \| SP \| Deodorant \| S&V \| Exact | S&V Deodorant → FNA756-75 | Nov 1 | $10 | enabled (uploaded) |
| FNA \| SP \| Deodorant \| Discovery (auto) | Auto B&H → FNA755-75; Auto S&V → FNA756-75 | Nov 1 | $8 | enabled (uploaded) |

When the Oct 26 campaigns go live, pause the two legacy campaigns `500ml Body Wash - Scent Keywords` and `Body Wash Exact Match - Keywords` so they don't bid against the new ones.

## Keywords (all exact match unless noted)

**Body Wash B&H Exact:** bergamot body wash $2.00 · bergamot and hinoki body wash $1.75 · salt and stone bergamot hinoki body wash $1.50 · bergamot and hinoki $1.25 · hinoki body wash $1.25 · bergamot shower gel $0.75

**Body Wash S&V Exact:** santal body wash $2.00 · salt and stone santal vetiver body wash $2.00 · sandalwood body wash $1.75 · santal and vetiver body wash $1.75 · vetiver body wash $1.25 · sandalwood shower gel $0.75. Sandalwood terms sit with S&V (santal is sandalwood); they only converted before because the A&S bottle was being shown.

**Body Wash Men's Exact:** body wash men $1.25 · mens body wash $1.25 · body wash mens $1.10 · oil body wash $1.10. High-volume generic terms that converted in small data; hard $10 cap so they can't starve the scent campaigns.

**Lotion B&H Exact:** bergamot lotion $1.10 · bergamot body lotion $1.10 · bergamot and hinoki lotion $1.10 · bergamot and hinoki body lotion $1.10 · salt and stone lotion bergamot hinoki $1.00 · hinoki lotion $0.90 · scented body lotion $0.80

**Lotion S&V Exact:** santal lotion $1.10 · santal body lotion $1.10 · santal and vetiver lotion $1.10 · sandalwood body lotion $1.00 · sandalwood lotion for men $1.00 · salt and stone lotion santal $1.00 · mens body lotion $0.80 · body lotion for men $0.80 · salt and stone lotion $0.75 (test — cut after 20 clicks if it behaves like broad Salt & Stone body wash terms) · santal 33 lotion $0.70

**Deodorant B&H Exact:** bergamot deodorant $0.65 · bergamot and hinoki deodorant $0.65 · salt and stone deodorant bergamot $0.60 · salt and stone deodorant women bergamot $0.55 · hinoki deodorant $0.50

**Deodorant S&V Exact:** santal deodorant $0.65 · santal and vetiver deodorant $0.65 · sandalwood deodorant $0.60 · salt and stone deodorant santal $0.60 · salt and stone santal deodorant $0.60 · salt and stone deodorant women santal $0.55

Deodorant keywords are **seeds only** — there is still no deodorant keyword pull in `data/keywords/`. Run a Jungle Scout pull on competitor deodorant ASINs and add winners.

**Hand Wash Exact ($0.80 each):** B&H — bergamot hand soap, bergamot hand wash, hinoki hand soap, bergamot and hinoki hand soap. S&V — santal hand soap, vetiver hand soap, sandalwood hand soap, santal and vetiver hand soap. Scent-specific hand soap demand is small (~3K searches/month combined); generic `hand soap` (1.7M) and `luxury hand soap` are excluded.

Auto default bids: Body Wash and Lotion Discovery $0.60, Deodorant Discovery $0.40.

## Discovery negatives (campaign level)

- **Body Wash Discovery:** negative exact on every Body Wash exact keyword above, plus native body wash, luxury body wash, natural body wash, non toxic body wash, exfoliating body wash, antibacterial body wash, body wash women. Negative phrase: native, dove, old spice, dr squatch, irish spring, dial, method, aveeno, cetaphil, olay, eucalyptus, unscented, kids, baby.
- **Lotion Discovery:** negative exact on every Lotion exact keyword. Negative phrase: vaseline, aveeno, cetaphil, cerave, amlactin, eucerin, jergens, dove, gold bond, nivea, unscented, fragrance free, baby, kids, eczema.
- **Deodorant Discovery:** negative exact on every Deodorant exact keyword. Negative phrase: native, dove, degree, secret, old spice, lume, antiperspirant, clinical, kids, teen.

## Brand Defense

Shows our products on our own detail pages (scent-matched) and protects branded search. Bids $0.40–0.50.

| Ad group → SKU | Targets |
|---|---|
| Branded Keywords → FNA305-500, FNA605-500, FNA106-500, FNA755-75, FNA756-75 | exact: for and against, for and against body wash, for and against lotion, for and against deodorant, for and against shampoo; phrase: for and against |
| B&H Lotion on B&H pages → FNA605-500 | B&H body wash, deodorant, shampoo ASINs |
| B&H Body Wash on B&H pages → FNA305-500 | B&H lotion, deodorant, shampoo ASINs |
| S&V Shampoo on S&V pages → FNA106-500 | S&V body wash, deodorant ASINs |
| B&H Hand Wash on B&H pages → FNA505-500 | B&H body wash, lotion, deodorant ASINs |
| S&V Hand Wash on S&V pages → FNA506-500 | S&V body wash, deodorant, shampoo ASINs |

S&V Lotion was removed from Brand Defense because FNA606-500 is currently ineligible for advertising. Add it back (and add its product ads to the two S&V lotion ad groups) once the listing is fixed.

## Upload history

- **First upload (full file, 232 rows):** the five Oct campaigns failed with "Start date cannot be in the past" (start was set to Oct 19, uploaded after). The six Nov 1 Lotion and Deodorant campaigns were created; only the two FNA606-500 product ads failed ("Product is ineligible for advertising").
- **Re-upload (Oct 26 start):** Body Wash ×4, Brand Defense, and the new Hand Wash campaign.
- Bulk upload rule of thumb: start dates must be today or later at upload time, and a campaign can't have its start date changed once it has started — pause and re-enable instead.

## Managing keywords

- Pull the Search Term report weekly. Terms with 2+ orders at or under target ACoS coming through Discovery get promoted to exact in the matching scent campaign and added as negative exact in Discovery.
- Give a new keyword at least 14 days and 20 clicks before judging it. 20+ clicks with zero orders: pause it or negate the search term.
- Never negate our own scent or brand words (hinoki, bergamot, santal, vetiver, for and against) — zero orders there is a listing or stock problem, not a keyword problem.
- One round of changes per week; don't touch a keyword changed in the last 7 days.
- Before raising bids on a SKU, check FBA cover. Pause a SKU's campaigns about a week before it runs out rather than letting ads chase the last units.

## Open items

- FNA306-500 (S&V body wash) restock: enable S&V Exact, the S&V Discovery ad group, and move Men's to FNA306-500.
- FNA606-500 (S&V lotion): resolve ineligibility, then add product ads and the Brand Defense ad group.
- Deodorant keyword pull (Jungle Scout).
- Hand soap + lotion scent bundles would unlock `hand soap and lotion set` (~10K searches/month) and holiday gift terms.
- Later in the plan: Sponsored Brands scent collections (Nov 10), Sponsored Display retargeting (Nov 15), S&V refill campaign, January profit mode.
