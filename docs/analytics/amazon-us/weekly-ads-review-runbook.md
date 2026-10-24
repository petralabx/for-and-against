# Weekly Amazon Ads Review – Agent Runbook

Oct 24, 2026 · Stephen Alton · Live doc: https://claude.ai/artifact/N3yUsLY2CPLk2vhYLmCkNw

For & Against, Amazon US. A weekly, read-only deep dive of the Sponsored Ads account that ends in a reviewer-approved change list, with every past recommendation and its outcome visible before anyone decides.

## Purpose and operating rules

Every week the agent gathers the full picture of the ads account and the things that govern ad performance (stock, margin, conversion, reviews), compares it with what was recommended and decided in earlier weeks, and proposes a specific change list. A human approves, rejects or defers each change before anything goes live.

1. **Read-only by default.** The agent never changes a bid, budget, keyword, campaign state, price or listing. Its output is a proposal. Implementation happens only after a named reviewer approves, and is logged.
2. **Every recommendation carries its evidence.** Each proposed change states the metric, the window, the number, the threshold it crossed and the expected effect, so the reviewer can check it in under a minute.
3. **Memory is mandatory.** Before analysing, the agent reads the decision ledger (all prior weeks) and reports, for every open or implemented recommendation, what happened since. No recommendation is re-proposed without saying it was proposed before, what was decided, and why it is back.
4. **Stock and margin gate everything.** No increase in spend is proposed on an ASIN that is out of stock, has under 21 days of supply, or whose break-even ACOS is below the campaign's running ACOS.
5. **One window for decisions, one for context.** Changes are judged on the trailing 14 days (ending two days ago, because Amazon data lags); the trailing 60 days give the trend. Keyword-level decisions need a minimum sample (see Step 3).
6. **No fabricated numbers.** If a report did not load or a field was blank, the brief says so and the related recommendation is marked "insufficient data".

## Access and inputs

The agent needs a logged-in Seller Central session (sellercentral.amazon.ca, For & Against, marketplace set to United States) and the linked Amazon Ads console (advertising.amazon.com, entity ENTITY11CD4SJVG3H69). Today this means driving Chrome on a linked computer; if the Amazon Selling Partner connector or the Ads API is connected later, the same data can be pulled without a browser and the steps below map one-to-one.

| System | What it provides | Window the agent uses |
| --- | --- | --- |
| Ads console – Campaign Manager | Campaign, ad group, target, placement and search-term performance; bidding settings; negatives; scheduled campaigns | Trailing 14 days and trailing 60 days, both ending two days ago |
| Ads console – Sponsored ads reports | Search Term report (SP), Placement report, Targeting report, Advertised Product report, Purchased Product report | Same windows; Purchased Product report is what shows refill/lotion halo from body-wash ads |
| Seller Central – Business Reports | Sessions, conversion, featured-offer share and sales by child ASIN; weekly sales trend | Trailing 14 days by ASIN; weekly series back 20 weeks |
| Seller Central – Profit Analytics | Net proceeds, fees, ad cost and COGS per ASIN | Trailing 30 days; sets break-even ACOS per ASIN |
| Seller Central – Manage FBA Inventory and Manage Shipments | On-hand, inbound, days of supply, delivery promise, shipment status | Current snapshot |
| Brand Analytics – Search Query Performance | Query volume, brand impression/click/cart/purchase share | Latest complete week and latest complete month |
| Brand Analytics – Customer Loyalty | Repeat purchase rate, cart abandoners | Latest week |
| Seller Central – Voice of the Customer and customer reviews | NCX rate per ASIN, new critical reviews, rating changes | Current snapshot; reviews since last run |
| Amazon.com detail pages | Live price, delivery date, rating, who is advertising on the page, Buy Box | Current snapshot for the top 6 ASINs |
| The decision ledger (companion tab or shared sheet) | Every prior recommendation, decision, implementation date and outcome | All history |

Date windows are chosen so the Monday run looks at data through the previous Friday. Amazon's sales and ads data are complete only after ~48 hours, and search-term reports can restate for up to 72 hours.

## Step 1 – Data collection checklist

Run in this order; each item names the page, the window and the fields to capture. Save every table as a dated CSV or sheet tab so the week can be re-audited.

### A. Context the ads depend on (Seller Central)

- [ ] **Decision ledger** – read all rows; list every recommendation with status Approved-not-yet-implemented, Implemented-awaiting-result, Deferred, or Rejected-with-revisit-date.
- [ ] **Manage FBA Inventory** – for every active SKU: on-hand, inbound, days of supply (on-hand and total), last-30-day sales, listing status, restock flags, AWD errors. Flag anything under 21 days on hand or at zero.
- [ ] **Manage Shipments** – every shipment not Closed: ID, created date, status, delivery window, units expected/located, last event. Flag anything past its window with no FC scan.
- [ ] **Amazon.com detail page** for the top 6 ASINs by sessions – price, delivery date shown, rating and review count, Buy Box holder, which brands' ads appear on the page, whether Subscribe & Save shows.
- [ ] **Profit Analytics**, last 30 days, per ASIN – net sales, units, selling fees, fulfillment, storage, inbound, ad cost, COGS, net proceeds. Compute contribution per unit before ads and break-even ACOS per ASIN.
- [ ] **Business Reports – Detail Page Sales and Traffic by Child Item**, trailing 14 days and prior 14 days – sessions, page views, featured-offer %, units, unit session %, sales. Also the **Sales and Traffic by week** series, last 20 weeks.
- [ ] **Voice of the Customer** – NCX rate, NCX orders/orders and top reason per ASIN; note any ASIN that moved to Fair or Poor.
- [ ] **Customer reviews** – new reviews since the last run on the advertised ASINs (star, scent, text); rating change per ASIN.
- [ ] **Brand Analytics – Search Query Performance** (brand view, latest complete week, plus latest month for trend) – top 50 queries: volume, impression/click/cart-add/purchase counts and shares.
- [ ] **Brand Analytics – Customer Loyalty** – repeat purchase rate, repeat orders, cart abandoners, new-to-brand share.
- [ ] **Subscribe & Save** – enrolled ASINs, discount level, subscriber count and S&S share of sales if shown.

### B. The ads account (Ads console)

- [ ] **Campaign list**, trailing 14 and 60 days – for every campaign (all states, not archived): type, targeting, status, start date, daily budget, budget-exhausted days, bidding strategy, placement adjustments, impressions, clicks, CTR, CPC, spend, orders, sales, ACOS, top-of-search impression share.
- [ ] **Ad groups** – per campaign: default bid, number of targets, products advertised and their stock status (cross-check with A).
- [ ] **Targets (keywords and product targets)** – per ad group: target, match type, state, bid, impressions, clicks, spend, orders, sales. Both windows.
- [ ] **Search terms** – per ad group, trailing 14 and 60 days: customer search term, matched target, match type, bid, impressions, clicks, spend, orders, sales. Export the full list, not the first page.
- [ ] **Placements** – per campaign: top of search, rest of search, product pages: impressions, clicks, spend, orders, sales. Both windows.
- [ ] **Negative targeting** – per campaign and ad group: every negative keyword and negative ASIN with its match type.
- [ ] **Scheduled and paused campaigns** – anything with a future start date or paused in the last 30 days, with its settings, so the reviewer sees what is about to change.
- [ ] **Sponsored ads reports** – generate and download the SP Search Term report (60 days), Placement report, Advertised Product report and Purchased Product report; the last one identifies orders of a different ASIN than the one advertised (refill or lotion halo).
- [ ] **Sponsored Brands and Sponsored Display** – if any exist: same campaign, target and placement capture; brand-search and new-to-brand metrics.
- [ ] **Budget and billing** – month-to-date spend against the monthly cap the reviewer set; any budget-rule or schedule-rule in place.
- [ ] **Change history** – the campaign History tab for each live campaign since the last run, to confirm that approved changes were implemented as approved and that nothing else changed.

### C. Competitive snapshot (light, weekly; deep, monthly)

- [ ] For the five core queries (bergamot body wash, santal body wash, body wash refill, bergamot lotion, santal shampoo): who holds sponsored top-of-search and the first organic positions, their price and rating. Note any new entrant or price move on Salt & Stone, Nécessaire, ATTITUDE, Native.

## Step 2 – Analysis

The agent computes the following from the collected data, in this order, and keeps the working tables for the appendix of the brief.

1. **Reconcile last week.** For each recommendation in the ledger that was Approved: confirm in campaign History that it was implemented, when, and exactly as written. For each Implemented recommendation 14+ days old: compare the metric it targeted before and after (same-length windows) and record the outcome as Worked / Didn't / Too early / Confounded (stockout, price change, holiday).
2. **Account scorecard.** Spend, sales, orders, ACOS, TACOS (ad spend / total sales), CPC, CTR, conversion, top-of-search share for the trailing 14 days versus the prior 14 and the 60-day average. State whether each is inside the targets set by the reviewer (currently: ACOS ≤ 30% on bottles, ≤ 35% on refills; TACOS ≤ 12%).
3. **Break-even by ASIN.** From Profit Analytics: contribution per unit before ads ÷ average price = break-even ACOS. Any campaign or target running above the break-even of the ASIN it advertises is loss-making and is flagged, with the dollar loss.
4. **Stock gate.** Join every advertised ASIN to on-hand, days of supply and the live delivery date. Any ASIN at zero, under 21 days, or showing a delivery promise beyond 7 days is marked "do not fund"; its spend in the window is reported as avoidable.
5. **Placement economics.** Per campaign and in total: spend, orders, sales, ACOS and conversion by placement. Flag any campaign where product pages exceed 40% of spend or run above 1.5x the ACOS of search placements.
6. **Target efficiency.** Per keyword/product target, both windows: spend, orders, ACOS, cost per order, conversion. Classify: Scale (ACOS ≤ target, ≥ 3 orders), Hold, Reduce (ACOS 1–2x target), Pause (≥ 15 clicks and 0 orders in 60 days, or ACOS > 2x target on ≥ 3 orders), Insufficient data.
7. **Search-term mining.** From the 60-day search-term export: (a) terms with ≥ 2 orders and ACOS ≤ target not yet an exact keyword → harvest candidates with the suggested bid (observed CPC × 0.9); (b) terms with ≥ 10 clicks and 0 orders, or ACOS > 2x target on ≥ 2 orders → negative-exact candidates; (c) the same term matched by two or more targets → duplicate, keep the cheaper one; (d) brand terms (ours and competitors') → tagged for the branded and conquest campaigns.
8. **Bid diagnostics.** Realized CPC versus set bid per target; where CPC > bid by 20%+ under up-and-down bidding, note the inflation. Suggested bid per target = target ACOS × price × conversion rate (60-day, with the ASIN's conversion as the floor for thin data).
9. **Budget diagnostics.** Days each campaign hit its budget; share of spend by campaign versus share of orders; hours-of-day pattern if available. Reallocation proposals move budget from campaigns above target to campaigns below target with headroom, never raising total monthly spend without a reviewer decision.
10. **Structure checks.** Duplicate targets across campaigns, mixed-ASIN ad groups serving scent-specific keywords, broad/phrase keywords carrying most of a campaign's spend, campaigns with no negatives, missing branded campaign, missing defensive product-targeting on own ASINs, SB/SD gaps.
11. **Organic and halo.** From SQP: brand purchase share on the five core queries week over week; from the Purchased Product report: share of ad-attributed orders that were a different ASIN (refill, lotion). From Business Reports: organic conversion by ASIN versus ad conversion; if ad conversion is under half of organic, the targeting is off.
12. **Experience risk.** New critical reviews, rating drops, NCX changes on advertised ASINs; anything that moved is flagged against the ASIN's spend, because ads amplify a bad page.
13. **Anomalies.** Any metric that moved more than 30% week over week is explained (stockout, price, competitor, seasonality, budget cap) or marked unexplained.

## Step 3 – Recommendation rules

Analysis becomes a change list only through these rules. The reviewer owns the thresholds; the agent applies them and shows its work. Starting values reflect the Oct 24 review and should be tuned after four weeks of outcomes.

| Trigger (trailing 14 days unless stated) | Recommendation | Evidence the brief must show |
| --- | --- | --- |
| Advertised ASIN at 0 on hand, < 21 days of supply, or delivery promise > 7 days | Pause its ads or cut to a $5/day holding budget until in stock | On-hand, inbound ETA, live delivery date, spend in window |
| Campaign ACOS > 1.5x target on ≥ 10 orders | Reduce bids 15–25% on its targets above target; move product-page share down | Campaign and placement table |
| Target with ≥ 15 clicks and 0 orders in 60 days | Pause target | Target row, both windows |
| Target ACOS > 2x target on ≥ 3 orders (60 days) | Pause, or bid − 30% if it is a core scent term | Target row, cost per order |
| Target ACOS ≤ target on ≥ 3 orders and < 90% impression share or budget-capped | Bid + 10–20% or budget + 20%; never both in the same week | Target row, TOS share, budget-hit days |
| Search term ≥ 2 orders, ACOS ≤ target, not an exact keyword | Harvest as exact in the matching scent campaign at observed CPC × 0.9; add as negative-exact in the source broad/auto | Search-term row |
| Search term ≥ 10 clicks, 0 orders, or ACOS > 2x target on ≥ 2 orders | Negative-exact at ad-group level (campaign level if it recurs across ad groups) | Search-term row, matched target |
| Same search term served by two or more targets or campaigns | Negative it where it is dearer; keep the cheaper match | Both rows with cost per order |
| Product-page placement > 40% of campaign spend and ACOS > 1.5x search placements | Switch to down-only bidding, lower base bid, add top-of-search modifier | Placement table |
| Realized CPC > bid + 20% | Switch bidding to down-only or fixed | Target CPC vs bid |
| Branded term converting in a non-branded campaign | Move to the branded campaign; negative elsewhere | Search-term row |
| Competitor's brand term with ACOS ≤ target on ≥ 2 orders | Keep in a dedicated conquest campaign with its own budget | Search-term row |
| Ad group advertising > 1 scent on scent-specific keywords | Split into one ad group per scent | Ad group products list |
| New campaign scheduled to start within 14 days | Review its bids, budget and overlap with live campaigns before launch | Settings and duplicate check |
| Advertised ASIN rating drop ≥ 0.2, or NCX rate > 8% | Hold spend flat; raise a product/listing flag to the owner | VOC and review rows |
| Refill or lotion ASIN with conversion ≥ 15% and ACOS ≤ target | Shift budget toward it from any bottle campaign above target | Business Report and campaign rows |
| Monthly spend on pace to exceed the reviewer's cap | Propose which campaigns to throttle | Month-to-date spend |

Guardrails the agent applies to its own proposals: no single bid change larger than 30%; no budget change larger than 30% in a week; no more than 25 negatives added per campaign per week without a reviewer note; total weekly spend change capped at ±20% unless the reviewer asks for more; a recommendation rejected in the last 4 weeks is re-raised only with new evidence and a note of the prior rejection.

## Step 4 – The reviewer package

The agent publishes one weekly brief as a doc (dated, e.g. "Ads Review – week of Nov 2"), in this fixed order so the reviewer always knows where to look. Target length: readable in 15 minutes, with the full tables in an appendix.

1. **Scorecard (one table).** Spend, sales, orders, ACOS, TACOS, CPC, conversion, TOS share: this 14 days vs prior 14 vs 60-day average, each marked in/out of target. One sentence on the biggest mover and why.
2. **What happened to last week's decisions.** Every recommendation from prior briefs that is open or recently implemented, with its outcome so far. This section comes before any new recommendation so the reviewer sees the track record first.
3. **Blockers outside ads.** Stock, shipments, delivery promise, rating or NCX changes that constrain this week's choices, each with the ASINs affected.
4. **Proposed changes (the change list).** One row per change: ID, campaign / ad group / target, current value, proposed value, rule triggered, evidence (the numbers), expected effect ($ and ACOS), risk, and a Decision column for the reviewer (Approve / Reject / Defer / Modify) plus a Notes cell. Ordered by expected dollar impact. Grouped under: Pause and stop-loss; Negatives; Bids; Budgets; Harvests and new keywords; Structure; Flags for other owners.
5. **Things the agent chose not to recommend.** Items that triggered a rule but were held back by a guardrail, a prior rejection or thin data, so the reviewer can overrule.
6. **Watch list for next week.** Targets and campaigns approaching a threshold; experiments running and when they can be judged.
7. **Appendix.** The full campaign, target, placement and search-term tables for both windows; the stock join; the per-ASIN break-even table; the SQP top-50; links to the saved exports.

Each change row is written so it can be executed verbatim by whoever implements it ("Campaign 500ml Body Wash – Scent Keywords › ad group › keyword 'mens body wash' (exact): state Enabled → Paused"). Dollar estimates use the 14-day run rate scaled to 30 days and are labelled as estimates.

## Step 5 – The decision ledger

The ledger is the memory. It lives in one place (a tab of the live doc, or a shared sheet the agent can read and append to), one row per recommendation, never deleted. The agent reads it in full at the start of every run and appends the new rows at the end; the reviewer fills the Decision column; whoever implements fills the implementation date; the agent fills Outcome from the data two weeks later.

| Column | Content |
| --- | --- |
| ID | YYYY-WW-nn (week of the brief, sequence) |
| Date proposed | Brief date |
| Scope | Campaign › ad group › target, or account-level |
| Change | Current → proposed, written to be executed verbatim |
| Rule | Which Step 3 rule triggered it |
| Evidence | The numbers and window at proposal time |
| Expected effect | $/month and ACOS, as estimated |
| Decision | Pending / Approved / Modified / Deferred / Rejected |
| Reviewer and note | Who decided and why, especially for Rejected and Modified |
| Implemented on | Date, and by whom; blank until done |
| Outcome (at +14 days) | Not implemented / Too early / Worked / Didn't work / Confounded, with the before-and-after numbers |
| Related IDs | Earlier proposals of the same change, so the reviewer sees the history in one hop |

Rules for the ledger: a change re-proposed after a rejection cites the rejected ID and what is new; "Deferred" rows carry a revisit date and reappear automatically in that week's brief; outcomes are judged on the metric named in the proposal, over equal windows before and after, and are marked Confounded when a stockout, price change, holiday or another change on the same target overlaps. Every 4 weeks the agent adds a one-table summary: proposals by rule, approval rate, and share of implemented changes that Worked — that is how the thresholds in Step 3 get tuned.

### Seed entries from the Oct 24, 2026 review

| ID | Scope | Change | Rule | Evidence (Sep 24 – Oct 23) | Expected effect | Decision | Outcome |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 2026-43-01 | Scent Keywords and Exact Match campaigns › B&H and S&V body wash ads | Pause until on-hand > 0 and delivery date normal | Stock gate | 0 on hand; delivery Nov 16–Dec 17; ~$1,160 spend on B&H; 185% ACOS last 7 days | Saves ~$35/day; removes loss-making spend | Pending | Not implemented |
| 2026-43-02 | Both campaigns › bidding strategy | Dynamic up-and-down → down-only; base bids $0.80–$1.00; top-of-search +50–100%; product pages 0% | Placement > 40% spend, CPC > bid + 20% | Product pages 55% of spend at 163% ACOS vs search 79%; CPC $1.88 vs $1.50 bids | Shifts mix to search; est. ACOS 110% → 70–80% before other fixes | Pending | Not implemented |
| 2026-43-03 | Both campaigns › negatives | Add negative-exact: native body wash, body wash, body wash men, mens body wash, body wash women (on broad/phrase); pause 'vetiver body wash' broad | Search term ≥ 10 clicks 0 orders or ACOS > 2x | ~$700 spend, 17 orders across these terms; 'vetiver body wash' $232 for 7 orders at 168% | ~$500/month of spend removed | Pending | Not implemented |
| 2026-43-04 | Scent Keywords › 'body wash men' and 'mens body wash' exact; Exact Match › 'body wash men' | Pause (duplicate across campaigns; 219% ACOS) | Duplicate target; ACOS > 2x | $166 + $76 for 10 orders combined | ~$200/month removed | Pending | Not implemented |
| 2026-43-05 | Scheduled FNA campaigns (Oct 26) | Do not launch Men's Exact; pause old Scent Keywords when B&H Exact goes live; hold B&H and S&V Exact until stock received | Scheduled campaign review; stock gate | 4 duplicate keywords live in both; bids $1.75–$2.00 same as the 90–144% ACOS terms | Avoids self-competition and relaunching into a stockout | Pending | Not implemented |
| 2026-43-06 | New campaign | Branded exact campaign ('for and against', 'for & against body wash') at ~$0.50; SP product targeting on own ASINs | Branded term in non-branded campaign; no defense | 'for and against body wash' 6.6x ROAS; competitors advertising on own pages | Protects branded demand at low cost | Pending | Not implemented |
| 2026-43-07 | Budget allocation | Shift budget to S&V 2L refill, B&H lotion, A&S bottle; 30% ACOS target per campaign | Refill/lotion CVR ≥ 15% | Refill 24.6% CVR, lotion 18.6%, bottles 5% | Same orders at 30% ACOS ≈ $1,150/month | Pending | Not implemented |

## Step 6 – Human review and go-live

Nothing changes in the Ads console until a named reviewer has marked each row. The loop is: agent proposes (Monday) → reviewer decides (by Tuesday) → implementer applies approved rows (Tuesday/Wednesday) → agent verifies next Monday from campaign History and records outcomes two weeks on.

### Reviewer checklist (per brief, ~15 minutes)

- [ ] Read the scorecard and the "what happened to last week's decisions" section first; note any implemented change whose outcome is Didn't work or Confounded.
- [ ] Check the blockers: is any proposed increase on an ASIN that is out of stock or has a product flag? If so, reject it.
- [ ] For each change row: does the evidence support the rule? Is the expected effect plausible against the 60-day numbers? Mark Approve / Reject / Defer / Modify and add a note for anything other than Approve.
- [ ] Look at "things the agent chose not to recommend" and overrule where you disagree.
- [ ] Confirm the monthly spend cap and the ACOS targets still stand, or change them in the brief so next week's run uses the new values.
- [ ] Approve the brief as a whole; the approved rows become the implementation list.

### Implementation and verification

- The implementer applies only Approved and Modified rows, exactly as written (or as modified), in one session, and enters the implementation date in the ledger. Anything that could not be applied is marked with the reason.
- The agent, next run, reads campaign History for every live campaign and reconciles: each approved change found as implemented, any change found that was not in the approved list (flagged as unlogged), and any approved change missing (flagged as not implemented).
- Outcomes are recorded at +14 days on the metric the proposal named; the reviewer sees them in section 2 of the next brief.
- If the Ads API or an Amazon-approved write tool is connected later, the implementation step can be delegated to the agent for Approved rows only, still logged, still after human approval; nothing in this runbook changes except who clicks.

### Escalation

- Any week where ACOS is above 2x target, spend is above 120% of the monthly pace, or an advertised ASIN has gone out of stock gets a same-day flag to the reviewer rather than waiting for Monday.
- Product, listing, pricing and inventory findings are flagged to their owners in the brief; they are not ads decisions and the reviewer only acknowledges them.

## Cadence and the monthly deep dive

| When | What runs | Output |
| --- | --- | --- |
| Every Monday, 7:00 ET | Steps 1–4 on data through the previous Friday; ledger reconciliation and outcome scoring | Weekly brief + ledger rows |
| Tuesday by end of day | Reviewer decisions | Ledger Decision column filled |
| Wednesday | Implementation of approved rows | Implementation dates in ledger |
| First Monday of the month | Weekly run plus: full 60-day search-term harvest and negative sweep; per-ASIN break-even refresh from Profit Analytics; SQP monthly view for the core queries and refill share; competitor price/rating snapshot on the five core queries; campaign structure review (duplicates, mixed ad groups, missing campaign types); 4-week rule-tuning summary (approval rate, Worked rate by rule) | Monthly section appended to the brief; proposed threshold changes for the reviewer |
| Ad hoc (same day) | Stockout of an advertised ASIN; ACOS > 2x target for 7 days; spend > 120% of monthly pace; rating drop ≥ 0.3 on an advertised ASIN | Flag to reviewer |

Seasonal note: holiday peak fulfillment fees (Oct 15–Jan 14) lower the break-even ACOS on every ASIN; the monthly break-even refresh must pick this up. Black Friday / Cyber Monday deal and budget decisions need a separate planning pass by early November.

## Setup before the first run, and open questions

- [ ] **Access path.** Decide how the agent reaches the data: (a) a scheduled Claude task that requires the linked computer and drives Chrome (works today, needs the Seller Central session to stay logged in and the computer awake); (b) the Amazon Selling Partner connector for inventory, sales and Profit Analytics plus the Ads API for campaign data (no browser, more reliable, needs authorization). (a) now, (b) when authorized.
- [ ] **Ledger home.** Pick where the ledger lives (the live doc's tab, or a shared sheet). The agent needs read and append access; reviewers need edit access to the Decision column.
- [ ] **Named roles.** Reviewer (ads decisions), implementer (applies approved rows), and owners for product/listing/inventory flags.
- [ ] **Targets and caps.** Confirm starting ACOS targets (30% bottles, 35% refills), TACOS target (12%), the monthly spend cap, and the minimum-sample thresholds in Step 3.
- [ ] **Exports folder.** A dated folder per run for the CSVs the agent saves, so briefs can be audited.
- [ ] **First-run baseline.** Run Steps 1–2 once without recommendations to produce the baseline tables and confirm every page loads for the agent; then seed the ledger (done above with the Oct 24 items).

Open questions for the reviewer:

1. Who is the reviewer and who implements? (Greg has been named for ads in conversation; confirm.)
2. Should Canada be added as a second marketplace run once the US loop is stable?
3. Is there a hard monthly ad budget, and does it include Sponsored Brands if those launch?
4. Should the agent be allowed to apply "stop-loss" rules (pause an out-of-stock ASIN's ads) without waiting for Tuesday, with an immediate notification instead of prior approval?
