# Channel: Amazon Attribution (tracking layer)

**Last verified:** 2026-08-11 · Links: [Amazon listings](amazon.md) · [paid ads](paid-ads.md) · [learnings](../learnings.md)

Amazon Attribution is the measurement layer underneath every off-Amazon traffic source (Webflow site, Google Ads, eventually creators/TikTok Shop). It exists because neither Google Ads' own conversion tag nor generic web analytics can see a purchase that completes on amazon.com — Amazon stopped supporting GCLID capture in 2022, and Google's conversion pixel can't fire on amazon.com at all. GA4 covers pre-click behavior only (page views, scroll, outbound clicks); Attribution is what closes the loop on the actual sale. The two are complementary, not redundant — see the note in `docs/analytics/` if/when that page exists.

Requires Amazon Brand Registry enrollment (already in place). Enrollment also unlocks the **Brand Referral Bonus** — roughly a 10% rebate on sales from Attribution-tagged links, paid out ~60 days later. The bonus only lands if the tag is firing correctly, so spot-check monthly rather than assuming it's working.

## Structure

**Campaign → ad group → tag**, and this is the part that determines how reporting rolls up later, so getting it right up front matters:

- **One campaign per traffic source.** Two currently exist:
  - `For & Against Website` — Webflow site traffic
  - `For & Against — Google Ads` — kept deliberately separate from the website campaign so Google-driven performance doesn't blend with organic/direct website traffic. See [paid-ads.md](paid-ads.md) for why.
- **Ad groups = placement or theme**, one per thing you want to measure separately.
- **Each ad group generates exactly one tag.** This is a hard platform constraint, not a design choice — if you need keyword-level or per-product routing *within* what would otherwise be one ad group, you create multiple ad groups for that theme instead (see the Google Ads pattern below).

### `For & Against Website` — current ad groups

| Ad group | Page |
|---|---|
| Homepage - Best Sellers | Homepage best-sellers section |
| Best Sellers Page | Dedicated best-sellers page |
| Shop All Page | Full catalog page |
| Shop By Scent - Hinoki & Bergamot | H&B scent collection page |
| Shop By Scent - Santal & Vetiver | S&V scent collection page |
| Shop By Scent - Amber & Sandalwood | A&S scent collection page |

Each scent gets its own ad group because each has a unique Webflow URL. Within each ad group, every product placed on that page gets its own tag — the same ASIN (e.g. H&B body wash) gets a **separate tag for every placement it appears in**, since a tag is placement-specific, not product-specific.

### `For & Against — Google Ads` — per-theme ad group pattern

Google Ads needs keyword-level Final URL routing, but Attribution only gives one tag per ad group — so instead of one `Salt & Stone` ad group, there are several sub-ad-groups per competitor/theme, each generating its own tag, applied at the keyword level in Google Ads:

- `Salt & Stone - Storefront`
- `Salt & Stone - Deodorant`
- `Salt & Stone - H&B Body Wash`
- `Salt & Stone - S&V Body Wash`

The ad group's default Final URL (storefront tag) is the fallback for any keyword that doesn't have a specific override. Full campaign/keyword mapping lives in [paid-ads.md](paid-ads.md).

## Naming convention

`[Page/Theme] - [Placement] - [Product/Scent]` — e.g. `Homepage Best Sellers - Body Wash - H&B`. Keep this exact pattern for every new ad group; it's what makes the weekly reconciliation join against Google Ads reports actually work (see [paid-ads.md](paid-ads.md) for that workflow).

## Product pool — campaign-level, not ad group-level

Attribution sets the trackable-product pool **at the campaign level**. All ad groups within a campaign share the same pool — there's no per-ad-group product restriction. Practical implication: add **every SKU across every placement** to the campaign pool up front, then only deploy the specific tags each page/ad actually needs. Unused generated tags cause no issues, so over-provisioning the pool is safe.

## URL handling rules

- **Never run an Attribution tag through a shortener or redirect** — both silently strip the tracking parameters, and the click will show up nowhere. This is the single most common way Attribution data goes missing.
- Leave any platform's own URL-parameter options (e.g. Google Ads Campaign URL Options) blank when an Attribution tag is already the Final URL — parameter conflicts can break the tag.
- **14-day, last-touch attribution window.** Don't judge a cohort's performance until 14 days after the traffic ran — a week-old cohort will look artificially weak while the window is still open.

## Known platform quirks

- **No native delete.** Once an ad group or tag exists, you can't delete it — archive or rename duplicates, and contact Amazon Advertising support if you need it actually removed.
- **The ad group creation wizard closes after the campaign is created.** To add more ad groups afterward, go through the campaign's detail page (or bulk upload) — there's no "add another" prompt in the original flow.
- **Product eligibility gaps.** A product can be flagged unavailable/ineligible for tracking (e.g. H&B Shampoo was flagged "Currently unavailable" at one point) even if it looks live elsewhere — check eligibility before assuming a generated tag will actually track.

## Open items / known issues

- A past audit found the homepage "Best Sellers" tagging didn't match the actual bestseller list (an Amber & Sandalwood body wash was tagged while H&B body wash/deodorant and S&V body lotion were not) — logged in [docs/learnings.md](../learnings.md); re-verify tag/product alignment periodically, especially after catalog changes like the A&S discontinuation.
- A mapping doc (tag name → tracking URL → Google Ads keyword) is recommended once the Google Ads ad group count grows past what's easy to track from memory — not yet built.
