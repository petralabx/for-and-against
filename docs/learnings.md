# Learnings & issue log

Append-only. Two sections: experiments (what we tested, what happened) and known issues (defects found in live surfaces). Agents: check the issue log before reusing any live copy.

## Known issues — live Webflow site (found 2026-08-18)

| # | Surface | Issue | Status |
|---|---|---|---|
| W1 | Hosting | `www` and apex both 200; two robots/sitemap hosts | OPEN — set default domain to apex |
| W2 | Shop + `/scents/*` | Buy on Amazon buttons are `href="#"` | OPEN — Stephen pasting Attribution tags |
| W3 | Scents template | Fragrance story paragraph is static Amber copy on every scent | OPEN — needs a CMS field |
| W4 | Footer | Legal links use `for-against.webflow.io`; About/Contact are `#` | OPEN |
| W5 | Products template | `/products/{slug}` 404s; not in sitemap | OPEN — decide if PDPs should exist |
| W6 | HTML head | No `rel=canonical` | OPEN — after www 301 |
| W10 | Legal pages (Returns, Privacy, Terms) — `Heading 10` | Main page title styled with `color: white` — likely invisible depending on the page background | OPEN — flagged during 2026-08-20 font audit, not fixed since it's a color issue, not a font issue |

## Known issues — site typography (found + fixed 2026-08-20, font audit)

Per `docs/brand/voice.md`: Headlines → Fortika, Sub-headlines → Inter Bold, Body → Inter Regular. Audit covered every page with unique content (Home, Shop All, all 5 category pages, Scents template, Best Sellers page, Contact, Returns/Privacy/Terms).

| # | Surface | Issue | Status |
|---|---|---|---|
| F1 | Site-wide | Base `<body>` tag defaulted to Arial, not Inter — root cause of most inconsistency below, since many text elements have no explicit font-family and fell through to this | FIXED — body tag font-family set to Inter |
| F2 | Homepage H1 ("Personal Care That Works With Your Body") | Rendered in Inter, not Fortika (headline) | FIXED — `.heading` class and its inner `.italic-text` span both set to Fortika |
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
| `Link Block 6` | Homepage Best Sellers section | Outline, white text, black border, no hover | Same base look (kept per Greg's preference) + added black-fill hover transition |
| `Link Block 7` | Shop All + all 5 category pages | Solid black background always, no hover, "amazon" word uncolored | Converted to outline/hover-fill pattern; "amazon" word now orange/bold/italic to match the homepage treatment |
| `Link Block 14` | Scents template CTA | Outline with hover-fill already (the "transition" look Greg specifically liked) | Unchanged except an explicit `transition` property added for guaranteed smoothness |
| `Link Block 16` | Dedicated Best Sellers page (`/best-sellers`) | Solid black background always, Arial font bug on both text spans | Converted to outline/hover-fill pattern; Arial → Inter |

**Resulting shared pattern:** transparent background, 1px solid `#1a1a1a` border, dark text by default (except `Link Block 6`, which keeps white text since it likely sits on darker product-card imagery), fills to `#1a1a1a` background with `#f5f2ec` text on hover, `background-color 0.2s ease, color 0.2s ease` transition. The orange/italic/bold "amazon" word treatment from the homepage button is now applied consistently on every button that includes it.

**Implementation note:** child text spans that need to change color on the parent's hover (e.g. "Shop Now") must have **no explicit color of their own** so they inherit the parent link's `color` property change — this is how `Link Block 14`'s existing hover already worked, and the pattern used to fix `Link Block 7` and `Link Block 16`. The "amazon" word spans keep an explicit static orange color that persists through hover unchanged.

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
