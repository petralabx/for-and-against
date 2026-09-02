# Learnings & issue log

Append-only. Two sections: experiments (what we tested, what happened) and known issues (defects found in live surfaces). Agents: check the issue log before reusing any live copy.

## Known issues — Scents page product grid (found + fixed 2026-08-25/26)

Greg reported the Amber & Sandalwood and Santal & Vetiver scent pages looking wrong after a prior session's button-alignment fix. What looked like several unrelated bugs turned out to trace back to one structural problem, discovered only after multiple false starts — logged in detail since the root cause (G4) is a genuinely non-obvious Webflow/CSS Grid trap worth knowing about before touching this grid again.

| # | Symptom | Root cause | Status |
|---|---|---|---|
| G1 | Product image squares stretched taller in some grid rows than others (most visible with a 2-item row next to a 4-item row) | `Image 12` had both `width:100%` and `height:100%` set alongside `aspect-ratio:1/1`. CSS `aspect-ratio` only fills a *missing* dimension — with both explicit, it became a no-op, so the image just tracked its parent's height. A same-session fix (button alignment) had given `product-card` `flex-grow:1`, so each row now stretches independently to its own tallest card, and the image inherited that variable per-row height instead of staying square. | FIXED — removed the conflicting `height:100%`, leaving `width:100%` + `aspect-ratio:1/1` to govern shape regardless of row height |
| G2 | Santal & Vetiver's heading rendered "Santal&Vetiver" with no spaces around the ampersand, on that scent's page only | Investigated at length: the CMS `name` field held correct spacing (verified twice, including after an explicit re-save), and the shared `Heading 12` style had no letter-spacing/word-spacing override. Root cause never independently confirmed — it's possible this was a downstream rendering symptom of G4 (the same broken grid pipeline corrupting unrelated nearby text), since it stopped reproducing once G4 was fixed and published. | Appears resolved as a side effect of the G4 fix; **not independently root-caused** — re-check if it recurs on any scent page |
| G3 | Product grid columns were visibly uneven width (bottle columns wider than pouch columns) despite `grid-template-columns: 1fr 1fr 1fr 1fr` looking nominally equal | Classic CSS Grid trap: a bare `1fr` track has an implicit minimum of `auto` (its content's min-content size), not `0`. Columns whose content had a wider natural min-content (longer wrapped product name/button text) were allocated more space than columns with narrower content, even though all four were "1fr." | FIXED — switched every column to `minmax(0, 1fr)`, forcing genuinely equal division regardless of content width |
| G4 | **Root cause of the whole saga.** After the G3 equal-column fix, the grid first showed severe text/button overflow, then — after further column-count changes — every single product card rendered stacked directly on top of every other card, regardless of how many columns were configured | `Collection List 9` had a stale `grid-template-areas` property (a 4-column × 2-row named-area map, `"Area"` through `"Area-7"`) left over from an earlier manual layout pass. **Named grid areas are fundamentally incompatible with a CMS-bound repeating Collection List** — Webflow has no way to assign a different named area to each dynamically-repeated item, so the items were very likely all competing for the same area, and the total overlap only became visually total once column-count changes stopped incidentally papering over it. This is why fixes G1–G3, all individually correct, kept producing confusing, seemingly-unrelated symptoms instead of resolving anything. | FIXED — removed `grid-template-areas` entirely and set `grid-template-rows` to plain `auto`, letting the grid place items via standard `grid-auto-flow` + explicit column count only. **This is the fix that actually mattered; everything before it was treating symptoms.** |
| G5 | Follow-up, post-G4: which column count actually looks right? | With the real bug fixed, 4 and 3 columns were each live-tested and both looked cramped in `pdp-left`'s ~420px available width (35% of a 1200px-max container) — text wrapping awkwardly, buttons crowded relative to the image. 2 columns was confirmed as the best fit. | DECIDED — base/`main` breakpoint set to 2 columns, matching `medium`/`small`; `tiny` stays 1 column |

**Lesson worth remembering:** never use `grid-template-areas` on a Webflow CMS Collection List. It can silently half-work (looks fine at a glance, especially with a small/particular item count) and then fail in ways that look like totally unrelated bugs — sizing, spacing, alignment — because the actual failure mode (items overlapping/misplaced) is easy to misread as something else, especially before checking under multiple column-count or breakpoint conditions. If a Collection List's grid ever behaves strangely in a way that doesn't match a straightforward CSS explanation, check for a leftover `grid-template-areas` first.

## Known issues — responsiveness (found + fixed 2026-08-20)

This project's Webflow breakpoints: Desktop (base, unbounded), Tablet (≤991px), Mobile L (≤767px), Mobile (≤479px) — no separate "laptop" breakpoint exists, so laptop-width screens render using the same base/Desktop styles as a large monitor.

### Shop All header image

| # | Issue | Fix | Status |
|---|---|---|---|
| R1 | Header image (`Section 12`, background-image) cropped badly on mobile — confirmed by Greg | First attempt: `tiny`-breakpoint `background-position` override to `50% 20%`. **Revised after Greg's screenshot evidence** — the source image is a wide banner combining a face close-up (left) with the "For & Against" wordmark (right); the original vertical-only adjustment still centered the crop on the eye. Changed to `75% 30%` (shifted horizontally toward the wordmark, not just vertically) | AWAITING Greg's next live confirmation |
| R7b | Scents template's own header (`Section 19`) — same source image | Same `75% 30%` mobile adjustment applied for consistency, on top of the earlier base `background-position: 50% 50%` fix (R7 below) | AWAITING confirmation |

### Shop By Scent template — comprehensive responsive pass

The entire template had **zero breakpoint overrides** on any of its core layout classes (`pdp-layout`, `pdp-left`, `pdp-right`, `products-grid`, `image-panel`, `scent-tabs`) before this session — a fixed desktop-only 35%/58% side-by-side layout at every screen size.

| # | Class | Issue | Fix |
|---|---|---|---|
| R2 | `pdp-layout` | No max-width; stretches awkwardly on large monitors, and the same base styles apply at both laptop and huge-monitor widths since there's no dedicated breakpoint between them | Added `max-width: 1200px` + auto margins to the base (Desktop) style |
| R3 | `pdp-layout`, `pdp-left`, `pdp-right` | Fixed `flex-direction: row` with 35%/58% widths and zero breakpoint overrides — would render as a cramped, illegibly narrow two-column layout on tablet and mobile | Tablet (`medium`): layout stacks to `flex-direction: column`, both columns to `width: 100%`, gap reduced to 32px. Mobile (`tiny`): padding tightened further to 16px, gap to 20px |
| R4 | `image-panel` | No explicit height — relied entirely on implicit row-layout height-matching from its sibling, which breaks once the layout stacks vertically | Added explicit height per breakpoint: 400px (tablet), 280px (mobile L and below) |
| R5 | `scent-tabs` | 40px gap between the three scent tab links, no wrapping — three scent names ("Amber & Sandalwood," etc.) likely overflow or crowd at narrower widths | Tablet: gap reduced to 20px, wrapping enabled, 16px side padding. Mobile: gap reduced further to 12px |
| R6 | `products-grid` — **real bug affecting every screen size, not just mobile** | `grid-template-columns: 1fr 1fr` was already defined on this class, but `display: flex` + `flex-wrap: nowrap` meant that grid definition was completely dormant — every product card was being squeezed into one non-wrapping flex row regardless of how many existed, at any screen size | Changed `display` from `flex` to `grid`, activating the pre-existing 2-column definition. Added a `small`-breakpoint override to drop to a single column on Mobile L and below |
| R6b | `products-grid` — **second bug found after R6, on scents with more than 4 products** | `grid-template-rows: auto auto` explicitly defined exactly **2 rows** — clearly built assuming exactly 4 products per scent (2×2). Santal & Vetiver has 6, and the overflow items rendered in visibly narrower, malformed cells (severe one-word-per-line text wrapping, a stray blue-colored `|` character) rather than wrapping into a clean 3rd row of 2 | Changed `grid-template-rows` from `auto auto` to plain `auto`, so any number of items wraps cleanly regardless of count. **Confirmed by Greg this did NOT fully resolve the issue** — see Open below |
| R7 | `Section 19` (this page's own header image) | Used the exact same source image as Shop All's header (`Section 12`) but with a very different, more extreme crop (`background-position: 0px 0px`, anchored top-left) — inconsistent with the rest of the site and likely also cropped badly | Changed base `background-position` to `50% 50%` to match Shop All's treatment; see R7b above for the subsequent mobile-specific revision |
| R8 | `Link Block 14` (Scents page "Buy on Amazon" button) — **regression introduced by this session's own earlier button-consistency work** | Had no explicit base `color` property — only a hover color had ever been set. When `Text Block 31` (its child text, a class shared with the homepage/Shop All buttons) had its own color stripped during the B1 hover-bug fix (see font-and-button-consistency PR), it lost its only color source on *this* button specifically, since the parent had nothing to inherit. Rendered as a plain unstyled blue hyperlink — confirmed via Greg's screenshot | FIXED — added `color: #1a1a1a` to `Link Block 14`'s base state, matching the rest of the unified button family. **Confirmed working by Greg on laptop** |
| R9 | `Image 12` (product card images on Scents template) | No fixed aspect ratio (`height: auto`) — each product photo rendered at whatever size its own source image's natural proportions dictated, making some cards' images visibly smaller/differently-shaped than others (worst on Refill Pouch products) | FIXED — added `aspect-ratio: 1 / 1` + `height: 100%`. **Partially effective** — improved consistency but the underlying R6b grid issue was still distorting cell width/sizing for overflow items independent of this fix |

**Open, unresolved, deferred to next session:**
- **R6b (products-grid overflow layout) not fully fixed** — the `grid-template-rows` fix did not resolve the malformed-cell issue on scents with more than 4 products (confirmed by Greg after republishing). Needs a fresh diagnostic pass, likely with direct visual/live inspector access rather than further Data-API-only guessing.
- **R10 (new) — `Link Block 14` reported "whited out" on mobile specifically**, inconsistent with the other buttons. Checked exhaustively via the Data API: no breakpoint overrides (`medium`/`small`/`tiny`) on `Link Block 14` at any pseudo-state (base, hover, active, focus), and the global `a` tag has no breakpoint overrides either — structurally this button should render identically at every screen size, contradicting what Greg is seeing live. Could not identify a technical cause through this API; likely needs a live mobile device inspector session (e.g. Safari's remote Web Inspector) rather than further backend-only checking.
- **R1/R7b (header image crop)** — second attempt made (`75% 30%`), not yet confirmed live.

**Recurring pattern this session:** several fixes confirmed correct via the Data API's own read-back did not match what actually rendered live for Greg (R1's first attempt, R6b, R10). Worth treating "confirmed via API" and "confirmed via live visual test" as two distinct, non-interchangeable levels of confidence going forward — the API accurately reflects what *will* be sent to the browser, but does not guarantee how it renders in every real-world context.

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
| W10 | Legal pages (Returns, Privacy, Terms) — `Heading 10` | Main page title styled with `color: white` — likely invisible depending on the page background | OPEN — flagged during 2026-08-20 font audit, not fixed since it's a color issue, not a font issue |

## Known issues — site typography (found + fixed 2026-08-20, font audit)

Per `docs/brand/voice.md`: Headlines → Fortika, Sub-headlines → Inter Bold, Body → Inter Regular. Audit covered every page with unique content (Home, Shop All, all 5 category pages, Scents template, Best Sellers page, Contact, Returns/Privacy/Terms).

| # | Surface | Issue | Status |
|---|---|---|---|
| F1 | Site-wide | Base `<body>` tag defaulted to Arial, not Inter — root cause of most inconsistency below, since many text elements have no explicit font-family and fell through to this | FIXED — body tag font-family set to Inter |
| F2 | Homepage H1 ("Personal Care That Works With Your Body") | Rendered in Inter, not Fortika (headline) | FIXED — `.heading` class and its inner `.italic-text` span both set to Fortika. **Note:** immediately after this fix, the Designer canvas rendered this specific heading's words with no spaces between them, despite the underlying text content being one normal string ("Personal Care That Works With Your Body") — confirmed not a font-wide issue since other Fortika headings render spacing correctly. Greg confirmed it looks correct in practice; logged as a likely Designer-canvas-only rendering quirk with the custom OTF file, not a real bug. |
| F3 | Homepage H2 ("Signature Scents. Thoughtful Formulas.") | Split-font bug — first half wrapped in a Fortika span, second half was a bare string inheriting Arial from body, so one heading rendered in two fonts | FIXED — `.heading-2` class itself set to Fortika, so the bare text now matches |
| F4 | Best Sellers page H1, Contact page H3 ("Contact Us") | No font styling applied at all (unstyled default elements) | FIXED — both given the existing `Heading 13` class (already Fortika) |
| F5 | Returns/Privacy/Terms (shared template, same element IDs across all three) — `Heading 10` | Main page title had no font-family | FIXED — set to Fortika |
| F6 | Returns/Privacy/Terms — `Heading 11` (6 recurring sub-section headers per page: "Returns," "Shipping," etc.) | No font-family or weight | FIXED — set to Inter, font-weight 700 (sub-headline spec) |
| F7 | `Text Block 15`, base `Text Block` class, `Text Block 66`/`Text Block 67` (Best Sellers page button text) | Explicit `font-family: Arial` overrides that survived even after the body tag fix | FIXED — all set to Inter |

**Already correct, no fix needed:** Find Your Scent heading, homepage Best Sellers section heading, Shop By Category heading, Scents template heading, and all Shop All/category page headings were already Fortika via their own class or an inner span.

**Not checked:** `Heading 4`, `Heading 5`, `Heading 9` style classes exist but weren't found rendering on any page checked (Home, Shop All, 5 category pages, Scents template, Products template, Best Sellers, Contact, Returns/Privacy/Terms) or inside the Nav Bar/Footer components — likely orphaned styles from earlier iteration. Left untouched since fixing unused styles has no visible effect; worth a cleanup pass someday if confirmed genuinely unused.

## Known issues — product button consistency (found + fixed 2026-08-20)

Four different "Buy on Amazon" button styles existed across the site with no shared pattern:

| Button class | Used on | Before | After |
|---|---|---|---|
| `Link Block 6` | Homepage Best Sellers section | Outline, black border, no hover; see B1 below for a deeper bug found here | Outline/hover-fill pattern matching the other three, plus B1 fixed |
| `Link Block 7` | Shop All + all 5 category pages | Solid black background always, no hover, "amazon" word uncolored | Converted to outline/hover-fill pattern; "amazon" word now orange/bold/italic to match the homepage treatment |
| `Link Block 14` | Scents template CTA | Outline with hover-fill already (the "transition" look Greg specifically liked) | Unchanged except an explicit `transition` property added for guaranteed smoothness |
| `Link Block 16` | Dedicated Best Sellers page (`/best-sellers`) | Solid black background always, Arial font bug on both text spans | Converted to outline/hover-fill pattern; Arial → Inter |

**Resulting shared pattern:** transparent background, 1px solid `#1a1a1a` border, dark text (`#1a1a1a`) by default, fills to `#1a1a1a` background with `#f5f2ec` text on hover, `background-color 0.2s ease, color 0.2s ease` transition. The orange/italic/bold "amazon" word treatment from the homepage button is now applied consistently on every button that includes it.

**B1 — Hidden per-element hover bug on `Link Block 6` (found + fixed 2026-08-20, after Greg tested live):** the initial fix only added a hover state to the button (`Link Block 6`) itself, assuming its "Shop Now" child text would inherit the color change. In fact the child (`Text Block 31`) had its own hardcoded `color: black` at rest **and its own separate `:hover` pseudo** (`color: #f5f2ec`) completely disconnected from the parent button — so hovering the button darkened the background, but the text only turned white if the mouse was directly over the text glyphs themselves, not anywhere else on the button. Fixed by moving the color logic onto `Link Block 6` (base `color: #1a1a1a`, hover `color: #f5f2ec`) and stripping `Text Block 31`'s own color entirely so it inherits from the ancestor's hover state correctly — the same pattern already used successfully for `Text Block 45`/`Text Block 67` on the other two buttons. **Confirmed `Text Block 45` and `Text Block 67` do NOT have this same hidden hover pseudo** — checked directly, only `Link Block 6`'s child had it.

**Implementation note:** child text spans that need to change color on the parent's hover (e.g. "Shop Now") must have **no explicit color of their own, in either the base or hover pseudo state** — any color rule on the child, even a hover-scoped one, will override or disconnect from the parent's own hover-driven inheritance. This is the root cause of B1 and worth checking for on any future button/card component with similar hover-fill patterns.

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
