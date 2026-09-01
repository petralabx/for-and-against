
# Channel: Amazon Attribution (tracking layer)

**Last verified:** 2026-08-20 · Links: [Amazon listings](amazon.md) · [paid ads](paid-ads.md) · [learnings](../learnings.md)

Amazon Attribution is the measurement layer underneath every off-Amazon traffic source (Webflow site, Google Ads, eventually creators/TikTok Shop). It exists because neither Google Ads' own conversion tag nor generic web analytics can see a purchase that completes on amazon.com — Amazon stopped supporting GCLID capture in 2022, and Google's conversion pixel can't fire on amazon.com at all. GA4 covers pre-click behavior only (page views, scroll, outbound clicks); Attribution is what closes the loop on the actual sale.

Requires Amazon Brand Registry enrollment (already in place). Enrollment also unlocks the **Brand Referral Bonus** — roughly a 10% rebate on sales from Attribution-tagged links, paid out ~60 days later. The bonus only lands if the tag is firing correctly, so spot-check monthly rather than assuming it's working.

## ⚠️ Structure superseded 2026-08-19 — read this first

The original version of this doc (2026-08-11) planned a **page/placement-level structure** — a separate ad group per site placement (Homepage Best Sellers, Shop All, each Scent page), with every product re-tagged per placement it appeared on. **That structure was never built.** During setup on 2026-08-19 we hit a hard platform constraint that makes it impractical:

- **Bulk upload only supports Google Ads and Facebook/Instagram as Publisher.** Webflow (or any owned-website publisher) is only available through the **manual** ad group creation flow — there is no bulk path for site placements.
- **Each manually-created ad group holds exactly one click-through URL.** An ad group can't hold multiple product URLs, so "one ad group per page" doesn't give per-product data within that page — it would require one ad group per product **per placement**, which for our ~20 live SKUs across 4-6 placements each meant 55-58 ad groups, all created by hand, with no bulk shortcut, ever (every new SKU or placement adds more by hand).

Given that cost, we deliberately traded placement-level granularity for a far smaller manual setup. **Page-level attribution is not available in this structure — if it's needed later, it has to come from site-side analytics (Webflow Analyze / GA tracking outbound clicks per page), compared side-by-side with Amazon's product-level numbers, not from Amazon Attribution reporting itself.**

## Current structure (product-level)

**Campaign → ad group → tag**, but ad groups now map 1:1 to **products** (or, for the storefront, a single browse destination), not placements:

- **Campaign:** `For & Against - Website Placement Tracking`
- **Publisher:** `Webflow`
- **Channel:** `Display` (closest fit — Amazon's 5 channel categories don't have a clean "owned website" option; Search implies a paid keyword ad, which this isn't)
- **One ad group per live product**, reused across every page that product appears on. Ad group naming: `[SKU] - [Product Name] - [Format] - [Scent]`, e.g. `FNA605-2LP - Body Lotion Refill Pouch - 2L - Bergamot & Hinoki`.

This means: Amazon Attribution reporting shows **clicks, detail page views, add-to-cart, purchases, and sales per product** (filtered to this campaign), but cannot tell you *which page* on the site generated a given click.

### Ad groups status (18 products + 1 storefront tag)

**Conditioner is explicitly out of scope for this round** (decided 2026-08-19) — the 2 Conditioner Refill Pouch tags (Bergamot & Hinoki `B0GR6NXNJK`, Santal & Vetiver `B0GR6VHYFD`) that were on the original manual checklist are **not being created**, and no Conditioner CMS work is planned until a future, separate pass. Scope is now 18 live products, and all 18 are confirmed generated with tags applied to the corresponding Webflow CMS `amazon-url` field as of 2026-08-19. This campaign can be considered complete for the current scope.

**Storefront tag added 2026-08-20:** a 19th ad group, `Storefront - Nav Bar`, was manually created (same Publisher: Webflow, Channel: Display pattern) with the click-through URL pointed at the Amazon storefront rather than a single product. Its tagged URL is applied to the "Buy On Amazon" nav bar button site-wide (a component default, so it updates every page that uses the nav bar, not just the homepage).

### `For & Against — Google Ads` campaign (unaffected)

Still runs the per-theme ad group pattern below, since Google Ads bulk upload *is* supported and doesn't hit the same constraint:

- `Salt & Stone - Storefront`
- `Salt & Stone - Deodorant`
- `Salt & Stone - H&B Body Wash`
- `Salt & Stone - S&V Body Wash`

The ad group's default Final URL (storefront tag) is the fallback for any keyword that doesn't have a specific override. Full campaign/keyword mapping lives in [paid-ads.md](paid-ads.md).

## Naming convention

- **Website campaign:** `[SKU] - [Product Name] - [Format] - [Scent]` per ad group, or a descriptive name (e.g. `Storefront - Nav Bar`) for non-product destinations.
- **Google Ads campaign:** `[Page/Theme] - [Placement] - [Product/Scent]`, unchanged from before.

## Product pool — campaign-level, not ad group-level

Attribution sets the trackable-product pool **at the campaign level**. All ad groups within a campaign share the same pool — there's no per-ad-group product restriction. Practical implication: add **every SKU** to the campaign pool up front, then only deploy the specific tags each product actually needs. Unused generated tags cause no issues, so over-provisioning the pool is safe.

## URL handling rules

- **Never run an Attribution tag through a shortener or redirect** — both silently strip the tracking parameters, and the click will show up nowhere. This is the single most common way Attribution data goes missing.
- Leave any platform's own URL-parameter options (e.g. Google Ads Campaign URL Options) blank when an Attribution tag is already the Final URL — parameter conflicts can break the tag.
- **14-day, last-touch attribution window.** Don't judge a cohort's performance until 14 days after the traffic ran — a week-old cohort will look artificially weak while the window is still open.

## Known platform quirks

- **No native delete.** Once an ad group or tag exists, you can't delete it — archive or rename duplicates, and contact Amazon Advertising support if you need it actually removed.
- **The ad group creation wizard closes after the campaign is created.** To add more ad groups afterward, go through the campaign's detail page (or bulk upload, where supported) — there's no "add another" prompt in the original flow.
- **Bulk upload publisher restriction (new, 2026-08-19):** only Google Ads and Facebook/Instagram are supported publishers for bulk upload. Any other publisher (including Webflow/owned website) must go through manual ad group creation, one tag at a time.
- **One click-through URL per manually-created ad group (new, 2026-08-19):** this is what makes page-level + product-level tagging simultaneously expensive — see the superseded-structure note above.
- **Product eligibility gaps.** A product can be flagged unavailable/ineligible for tracking (e.g. H&B Shampoo was flagged "Currently unavailable" at one point) even if it looks live elsewhere — check eligibility before assuming a generated tag will actually track.

## CMS coverage gap surfaced during 2026-08-19 setup

Three products had generated Attribution tags but **no corresponding Webflow CMS item**: Shampoo Refill Pouch (Bergamot & Hinoki, Amber & Sandalwood) and Hand Soap (Bergamot & Hinoki). Draft CMS items were created for all three with their tag attached; Greg is finishing these directly in Webflow (price, product image, SEO title/description, and adding a new "Hand Soap" Category option in Designer — the option list can't be edited via the Webflow MCP connector).

**Conditioner is out of scope for now** (see above) — no CMS work planned for that product line in this round.

## Open items / known issues

- 3 new draft CMS items (Shampoo Refill Pouch × 2, Hand Soap × 1) are being finished and published directly in Webflow by Greg — price, image, SEO copy, and the Hand Soap Category option.
- Conditioner (Refill Pouches and the base product line) is intentionally out of scope for this round — revisit as a separate, future pass rather than treating it as an open gap.
- 10 products remain not-live on Amazon at all (2 with no ASIN yet: Body Lotion 500ml Santal & Vetiver, Body Lotion Refill 2L Amber & Sandalwood); these need tags + CMS entries once launched, following the same product-level pattern above.
- A past audit (pre-2026-08-19) found the homepage "Best Sellers" tagging didn't match the actual bestseller list under the old (now-superseded) page-level plan — logged in [docs/learnings.md](../learnings.md). Not applicable to the new structure, but worth confirming the bestseller list itself is still accurate.
- A tag-name → tracking-URL → Google Ads-keyword mapping doc is still recommended for the Google Ads side once its ad group count grows past what's easy to track from memory — not yet built.
- A homepage category-tab filter for Shop All was attempted 2026-08-20 and paused — see `docs/sessions.md` same-date entry and `docs/learnings.md` for the platform constraints found (no custom code, no filter/conditional-visibility API).
