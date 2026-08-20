# Learnings & issue log

Append-only. Two sections: experiments (what we tested, what happened) and known issues (defects found in live surfaces). Agents: check the issue log before reusing any live copy.

## Known issues — Webflow Designer API constraints (found 2026-08-20)

Discovered while attempting to build an interactive category filter on the Shop All page. All three blocked a no-code/low-code single-page tab approach; noting them so a future session doesn't rediscover the same walls.

| # | Constraint | Detail | Status |
|---|---|---|---|
| A1 | Custom code (page freeform head/footer) | Writing any `<script>` via the Data API returns HTTP 406, even a one-line test script. Likely the site's Webflow plan doesn't include custom code (typically a CMS/Business-tier feature). CSS applied via Designer-native styling still works fine — only script injection is blocked. | OPEN — confirm plan tier with Webflow support or upgrade |
| A2 | Collection List query filtering | The native "Filter" panel (show only items where Category = X) is not exposed as a settable field via the Data API's `data_element_settings_tool`. Only settable manually in Designer's visual UI. | OPEN — no API workaround found; worked around, see below |
| A3 | Conditional visibility by Option-field equality | Element `visibility` can only bind to literal boolean CMS fields (e.g. Featured, Prime Eligible) via the API. Binding visibility to a computed condition like "Category equals Body Wash" (an Option field) is not exposed, even though Designer's own Conditional Visibility panel supports it natively. | OPEN — no API workaround found |

**Workaround shipped 2026-08-20:** rather than one page with a JS/conditional-visibility toggle, built 5 separate category pages (`create_page` with `duplicateOf` — this API action duplicates a page's full structure, nav bar, footer and all) each meant to hold a native Collection List Filter set manually in Designer. This sidesteps A1–A3 entirely since no custom code or API-level filtering is required — only a human click-through in Designer's Filter panel per page. See `docs/sessions.md` 2026-08-20 entries for both the original attempt and this resolution.

## Known issues — live Webflow site (found 2026-08-18)

| # | Surface | Issue | Status |
|---|---|---|---|
| W1 | Hosting | `www` and apex both 200; two robots/sitemap hosts | OPEN — set default domain to apex |
| W2 | Shop + `/scents/*` | Buy on Amazon buttons are `href="#"` | OPEN — Stephen pasting Attribution tags |
| W3 | Scents template | Fragrance story paragraph is static Amber copy on every scent | OPEN — needs a CMS field |
| W4 | Footer | Legal links use `for-against.webflow.io`; About/Contact are `#` | OPEN |
| W5 | Products template | `/products/{slug}` 404s; not in sitemap | OPEN — decide if PDPs should exist |
| W6 | HTML head | No `rel=canonical` | OPEN — after www 301 |
| W7 | Homepage — Find Your Scent | "Explore Scent →" button was `href="#"` and the tile image had no link at all | FIXED 2026-08-20 — button and a new dedicated tile-image link both bound to the Scents collection's `amazon-url` field (misleadingly named — its help text confirms it was always meant to hold this internal link, not an Amazon URL), now populated with the correct `forandagainstbodycare.com/scents/{slug}` link per scent. An earlier fix in this same session used a `collectionPage` link guess instead of this field — corrected once the intended field was discovered. |
| W8 | Homepage — Nav Bar | "Buy On Amazon" button linked internally to `/shop` instead of the Amazon storefront | FIXED 2026-08-20 — now points to the tagged storefront URL, opens in new tab, applies site-wide since it's a component default |
| W9 | Homepage — Shop By Category | All 4 category tiles (Body Wash, Body Lotion, Deodorant, Refill Pouches) had no link at all | FIXED 2026-08-20 — each tile now wraps its image in a dedicated link routing to its matching new category page (`/shop-body-wash`, `/shop-body-lotion`, `/shop-deodorant`); the Refill Pouches tile routes to `/shop` (unfiltered) since refills merge into their base category rather than having their own category — see the 2026-08-19 session's Conditioner/refill scoping decision. |

## Known issues — homepage Best Sellers carousel (found + fixed 2026-08-20)

Greg reported three distinct symptoms; only two were real bugs.

| # | Symptom | Root cause | Status |
|---|---|---|---|
| C1 | Scroll arrows invisible in Webflow Preview mode, but show fine on the published site | Expected Webflow behavior — the arrow icons come from an externally-loaded icon font (Tabler Icons via CDN in site head code); external font/CDN resources commonly don't render inside Designer's Preview sandbox even though they work normally in a real published page | NOT A BUG — no action taken |
| C2 | Desktop: scrolling right worked fine, but could never scroll all the way left — the leftmost product always stayed cut off, no matter how much you clicked/scrolled left | Two compounding causes: (a) the `.collection-scroll` flex container had `justify-content: center`, which is a well-known CSS trap — centering an overflowing flex container's content shifts the computed scroll-start point, making the true leftmost content unreachable via scrolling even though the right side scrolls normally; (b) separately, the scroll-left arrow button is an opaque, absolutely-positioned overlay sitting directly on top of the card content rather than beside it, so even at the correct scroll-left position the first ~40px of the leftmost card was hidden underneath the arrow | FIXED — (a) `justify-content` changed from `center` to `flex-start` on the `collection-scroll` combo class; (b) added 48px left/right padding to `Collection List 3` so card content clears the arrow overlay zone on both edges |
| C3 | Mobile: touch-scrolling didn't work on the carousel at all | The `collection-scroll` combo class had a `tiny` breakpoint override setting `overflow: visible`, which completely replaces the base class's `overflow: scroll` at mobile widths — disabling scroll entirely on phones | FIXED — removed the `tiny`-breakpoint `overflow` override so mobile inherits the base scrollable behavior |

**Confirmed working by Greg live on the published site** after both fixes (C2 fully resolved only after the `justify-content` fix — the padding fix alone did not resolve it, since the root cause was the flexbox centering trap, not just the visual overlap).

**Worth watching:** the `justify-content: center` + `overflow: scroll` combination (C2a) is an easy, non-obvious trap for any future horizontally-scrolling component on this site — check for it specifically if another carousel/scroller is ever added or reported as "can't scroll to the start."

## Known issues — live Amazon listings (found 2026-07-14, from source spreadsheet audit)

| # | SKU(s) | Issue | Status |
|---|---|---|---|
| 1 | FNA750/755/756/757/758-75 | All five deodorant titles end "(Hinoki & Bergamot)" regardless of actual scent | OPEN |
| 2 | FNA750–758-75 | All deodorant descriptions describe Amber & Sandalwood scent | OPEN |
| 3 | FNA606-500 | Santal & Vetiver lotion bullet 4 describes Bergamot & Hinoki scent | OPEN |
| 4 | FNA605-2LP, FNA606-2LP | Titles say "(Santal & Vetiv)" (truncated); FNA605-2LP is actually Hinoki & Bergamot | OPEN |
| 5 | FNA300-2LP, FNA305-2LP | Body wash pouch descriptions describe "Santal & Vetiver" regardless of scent | OPEN |
| 6 | Multiple shampoo/pouch | Typo "STREGTHENS" in bullets | OPEN |
| 7 | FNA305-2LP, FNA306-2LP | Title typos: "Beramot", "Satal" | OPEN |
| 8 | FNA500/505/506-500, FNA605/606-2LP | "All Natural" claim in titles — compliance risk, see docs/compliance/claims.md | OPEN |
| 9 | FNA606-500, FNA600-2LP | Missing ASINs in source sheet — confirm whether listed | OPEN |
| 10 | Source xlsx | SKU List vs Financials disagree on gallon vs 4-gallon labeling for FNAx0x | OPEN |
| 11 | Brand-wide | "Hinoki & Bergamot" (internal) vs "Bergamot & Hinoki" (live titles) — pick one | OPEN |

## Experiments

_(none logged yet — every copy/pricing/ad test gets an entry: hypothesis, change, result, decision)_
