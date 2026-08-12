# Amazon A+ Content — module template

**Last verified:** 2026-08-11 · Links: [amazon.md](amazon.md) · [amazon-attribution.md](amazon-attribution.md) · [claims policy](../compliance/claims.md) · [assets manifest](../../site/assets-manifest.md)

Standard A+ Content structure (free under Brand Registry, no Premium A+ approval needed) — built once for Santal & Vetiver Body Wash (SKU FNA300-500, ASIN B0GHT7YVVP) and designed to be templated across other scent/product listings by swapping accent color, copy, and imagery. **Do not build this for Amber & Sandalwood** — the line is being discontinued; extend to Hinoki & Bergamot body wash instead once assets are ready.

Amazon's accessibility guidance rejects modules with meaningful text baked into the image itself — copy always sits in the module's text fields, images stay visual-only.

## The 7-module arc

Sequenced deliberately: brand → hero moment → ingredient proof → scent story → cross-sell → sustainability/values → close.

### Module 1 — Standard Company Logo
**Spec:** 600 × 180px, PNG, transparent or brand-beige background
**Asset:** For & Against ampersand wordmark — a brand-level asset, reuse across the whole catalog, don't rebuild per listing.
**Tagline field:** "For natural. Against artificial."

### Module 2 — Standard Image Header with Text
**Spec:** 970 × 600px header image + up to 6,000 characters
**Asset:** Bottle in a real bathroom setting — marble counter, natural light, scent's accent Pantone visible in a towel/prop.
**Known gap:** existing lifestyle bathroom renders were built for a shampoo-box + bottle pairing, not a body-wash bottle alone — needs a dedicated re-render or reshoot before this module can go live for any scent.
**Headline:** "The Shower, Elevated."

### Module 3 — Standard Four Image/Text Quadrant
**Spec:** 4 images at 135 × 135px, up to 1,000 characters per tile (keep to 1–2 lines)
**Asset type:** simple icon/graphic tiles — a design job, not a photoshoot.
**Content pattern:** one hero active ingredient per tile (3 tiles) + one "Free From" claims tile.

### Module 4 — Standard Three Images & Text
**Spec:** 3 images at 300 × 300px, up to 1,000 characters per section
**Content pattern:** scent-forward storytelling — this is the single biggest content gap versus premium competitors, who spend real copy on fragrance mood.
**Known gap:** documented top/heart/base fragrance notes don't exist in project files for any scent — write at "mood" level (safe to publish) until a formulator supplies the real note breakdown, which would meaningfully strengthen this module.

### Module 5 — Standard Comparison Chart
**Spec:** up to 6 product columns, up to 10 metric rows
**Content pattern:** compare your own line's formats (Body Wash / Shampoo / Conditioner / Body Lotion / Deodorant / Hand Soap) rather than competitors — sidesteps Amazon's competitor-comparison restrictions entirely and doubles as the cross-sell engine.
**Rows:** format/use, size, key actives (per SKU — pull from `data/products.json` once actives are populated there), sulfate-free, paraben-free, dye-free, vegan.
**Native ASIN-linking** is available within this module specifically (standard A+ text modules elsewhere don't support hyperlinks) — use it to link each column to its ASIN, and consider a bundle column per the virtual-bundle cross-sell work.

### Module 6 — Standard Single Image & Sidebar
**Spec:** main image 300 × 400px + sidebar image 350 × 175px; ~500 characters main + ~500 characters sidebar; up to 8 sidebar bullets at 200 characters each
**Asset:** back-of-bottle ingredient panel shot + a texture/manufacturing-adjacent sidebar image.
**Compliance flag:** do not include an EWG ingredient-score claim in this module until compliance review clears it — see [claims policy](../compliance/claims.md). Copy is ready to drop in the moment that clears.

### Module 7 — Standard Image and Light Text Overlay (closer)
**Spec:** 970 × 300px background image, up to 300 characters
**Asset:** close, quiet product shot — bottle alone, warm light, negative space, matching the brand's minimalist retail artwork direction.
**Text:** "For natural. Against artificial. For fair pricing. Against markup. It's just that simple."

## Reuse checklist for a new scent/product

1. Swap accent color to the scent's Pantone (see `docs/brand/voice.md` or brand guidelines for the current scent → Pantone map).
2. Swap all copy to the new scent name and, where documented, its actual fragrance notes.
3. Confirm Module 5's comparison-chart actives are correct for that scent's SKUs — don't carry over S&V's placeholder blanks.
4. Re-check the Module 2 lifestyle-image gap above — don't assume an existing render covers the new product/scent combination.
5. Hold the EWG claim out of Module 6 until compliance sign-off, regardless of scent.

## Open items

- Module 5's per-SKU active-ingredient cells are blank for S&V pending confirmation from `For_Against_Product_Information.xlsx` — not yet resolved.
- No fragrance-note data exists for any core scent; Module 4 is running on mood-level copy across the board.
- Module 2's dedicated body-wash lifestyle shoot/re-render has not happened yet for any scent.
