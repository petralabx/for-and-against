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
