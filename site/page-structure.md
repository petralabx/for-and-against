# Site page structure

**Last verified:** 2026-08-20 · Site: forandagainstbodycare.com · Site ID: `6a3056618819e8c10dfe5b49`

## Pages in main nav

Home (`/`), Shop All (`/shop`), Shop By Scent (`/scents` template), Best Sellers (`/best-sellers`), plus static legal/contact pages. See [cms-schema.md](cms-schema.md) for the Scents/Products collection templates.

## Category filter pages (not in nav)

Added 2026-08-20 to give the homepage's "Shop By Category" tiles and Shop All's own category row somewhere internal to link to, without adding new items to the main nav. Each is a full duplicate of the Shop All page (`create_page` with `duplicateOf` — carries over the nav bar, footer, product grid, and styling automatically).

| Page | Slug | Page ID | Category filter |
|---|---|---|---|
| Shop - Shampoo | `/shop-shampoo` | `6a87538f49d3351c38f31055` | ✅ set manually by Greg in Designer: Category = Shampoo |
| Shop - Body Wash | `/shop-body-wash` | `6a87538f3a3149b2859ee694` | ✅ set manually by Greg in Designer: Category = Body Wash |
| Shop - Body Lotion | `/shop-body-lotion` | `6a875390782b6b6e51e31030` | ✅ set manually by Greg in Designer: Category = Body Lotion |
| Shop - Deodorant | `/shop-deodorant` | `6a875390770bfd4aa78dca1f` | ✅ set manually by Greg in Designer: Category = Aluminum Free Deodorant |
| Shop - Hand Soap | `/shop-hand-soap` | `6a875390f43cdac981cef9f9` | ✅ set manually by Greg in Designer: Category = Hand Soap |

**Filter status is per Greg's confirmation, not independently API-verified** — Collection List filter settings aren't readable via this Data API any more than they're writable, so there's no automated way to double-check. Worth a quick visual pass on each page before considering this fully closed out.

**Why these exist but aren't linked from nav:** intentional — these are filter destinations only, reachable from (a) the homepage's Shop By Category tiles, and (b) a small "Filter by category" link row added to Shop All and to each of these 5 pages themselves, not from primary site navigation.

**Refill Pouches has no dedicated page** — refill SKUs are categorized under their base product's Category (Body Wash Refill → Body Wash category, etc., per the 2026-08-19 session's decision), so the homepage's Refill Pouches tile links to Shop All unfiltered rather than a category that doesn't exist.

**SEO note (undecided):** these 5 pages have real, unique SEO titles/descriptions and will be crawlable/indexable by default like any other page — content overlaps with Shop All's product listings, which is a duplicate-content consideration worth a decision (noindex, canonical to Shop All, or leave as-is) before they're linked from anywhere prominent. Not implemented — the site's custom-code restriction (`docs/learnings.md` A1) blocks adding a meta-robots tag directly; Webflow's page-level "Disallow search engines" toggle may cover this if it exists in Designer's page settings (not exposed in this API's `update_page_settings` fields — worth checking manually).
