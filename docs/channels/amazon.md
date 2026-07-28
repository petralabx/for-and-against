# Channel: Amazon

**Last verified:** 2026-07-14 · Links: [products.json](../../data/products.json) · [claims](../compliance/claims.md) · [listing copy](../../copy/amazon-listings/)

## Structure

Amazon-first brand, US marketplace only, FBA fulfillment + AWD warehousing. Canadian marketplace expansion coming shortly (see [compliance](../compliance/claims.md) for CA requirements before launch). The Webflow site exists partly to push off-site traffic back to Amazon listings — Amazon's algorithm rewards listings that drive external traffic.

## Listing conventions (from live copy)

- **Title pattern:** `For & Against [Scent] [Product] | [benefit clause] | [ingredient clause] | [format/claims] | [size]`
- **Bullets:** ALL-CAPS lead phrase + em-dash/colon + benefit copy. Five bullets: scent story, ingredient story, value/manufacturing story, experience, brand story.
- **Description:** short declarative voice, ends with brand sign-off.
- **Backend keywords:** space-separated, cover all scent names across the line (cross-pollination), no commas needed, no competitor brand names.

## Hard rules

- **No competitor brand names anywhere in listing copy or backend keywords** — policy violation and suppression risk. The "$36 a bottle" framing without naming the brand is the approved ceiling.
- Retail prices in copy must match `data/products.json`; update the JSON first if pricing changes.
- Every listing copy change: PR review, then update the matching file in `copy/amazon-listings/` so the repo mirrors what's live.

## Known issues

The live listing copy has scent mix-ups and typos — check `docs/learnings.md` issue log before copying from any existing listing.
