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

## Known issues — responsiveness (found + fixed 2026-08-20)

This project's Webflow breakpoints: Desktop (base, unbounded), Tablet (≤991px), Mobile L (≤767px), Mobile (≤479px) — no separate "laptop" breakpoint exists, so laptop-width screens render using the same base/Desktop styles as a large monitor.

### Shop All header image

| # | Issue | Fix | Status |
|---|---|---|---|
| R1 | Header image (`Section 12`, background-image) cropped badly on mobile — confirmed by Greg | Added a `tiny`-breakpoint `background-position` override (`50% 20%` instead of dead-center `50% 50%`), shifting the crop point upward — the most common fix for this pattern. Could not visually verify the actual crop directly (network access to the asset host is blocked from this environment) | AWAITING Greg's live confirmation — may need further position tuning |

### Shop By Scent template — comprehensive responsive pass

The entire template had **zero breakpoint overrides** on any of its core layout classes (`pdp-layout`, `pdp-left`, `pdp-right`, `products-grid`, `image-panel`, `scent-tabs`) before this session — a fixed desktop-only 35%/58% side-by-side layout at every screen size.

| # | Class | Issue | Fix |
|---|---|---|---|
| R2 | `pdp-layout` | No max-width; stretches awkwardly on large monitors, and the same base styles apply at both laptop and huge-monitor widths since there's no dedicated breakpoint between them | Added `max-width: 1200px` + auto margins to the base (Desktop) style |
| R3 | `pdp-layout`, `pdp-left`, `pdp-right` | Fixed `flex-direction: row` with 35%/58% widths and zero breakpoint overrides — would render as a cramped, illegibly narrow two-column layout on tablet and mobile | Tablet (`medium`): layout stacks to `flex-direction: column`, both columns to `width: 100%`, gap reduced to 32px. Mobile (`tiny`): padding tightened further to 16px, gap to 20px |
| R4 | `image-panel` | No explicit height — relied entirely on implicit row-layout height-matching from its sibling, which breaks once the layout stacks vertically | Added explicit height per breakpoint: 400px (tablet), 280px (mobile L and below) |
| R5 | `scent-tabs` | 40px gap between the three scent tab links, no wrapping — three scent names ("Amber & Sandalwood," etc.) likely overflow or crowd at narrower widths | Tablet: gap reduced to 20px, wrapping enabled, 16px side padding. Mobile: gap reduced further to 12px |
| R6 | `products-grid` — **real bug affecting every screen size, not just mobile** | `grid-template-columns: 1fr 1fr` was already defined on this class, but `display: flex` + `flex-wrap: nowrap` meant that grid definition was completely dormant — every product card was being squeezed into one non-wrapping flex row regardless of how many existed, at any screen size | Changed `display` from `flex` to `grid`, activating the pre-existing 2-column definition. Added a `small`-breakpoint override to drop to a single column on Mobile L and below |
| R7 | `Section 19` (this page's own header image) | Used the exact same source image as Shop All's header (`Section 12`) but with a very different, more extreme crop (`background-position: 0px 0px`, anchored top-left) — inconsistent with the rest of the site and likely also cropped badly | Changed base `background-position` to `50% 50%` to match Shop All's treatment, plus the same `tiny`-breakpoint `50% 20%` mobile adjustment |

**Not yet visually confirmed** — Designer's connection timed out (idle disconnect) before a final visual pass could be done; all changes applied and verified correct via the Data API's own read-back, matching the pattern used throughout today's session. Greg to test live after publishing.

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
