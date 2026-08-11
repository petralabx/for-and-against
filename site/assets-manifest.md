# Visual asset manifest

**Last verified:** 2026-08-11 · Links: [cms-schema](cms-schema.md) · [products.json](../data/products.json) · [design tokens](../docs/design-system/) (colors only — do not edit that dir)

**Image files themselves are not committed to this repo.** They live in Webflow's Assets panel (site-rendering copies) and in this project's Claude knowledge base (working copies for content generation). This file is the index that lets a person or agent find the right asset without duplicating binaries into git history. Update this table whenever assets are added, replaced, or retired — don't let it drift from what's actually live.

**Known gap:** the Webflow Asset URL column is unfilled below — no Webflow connector was active when this manifest was built. Whoever has Webflow Assets-panel access should fill in the actual URLs (or connect Webflow so an agent can pull them directly).

## Manifest

| Filename (as known) | Scent | Product | Type | Likely purpose | Webflow Asset URL | Status / notes |
|---|---|---|---|---|---|---|
| `santal__vetiver_body_wash_front.jpg` | S&V | Body Wash | Packaging photo, front | Listing/PDP hero | TODO | — |
| `santal__vetiver_body_wash_front_1.jpg` | S&V | Body Wash | Packaging photo, front (variant) | Alt angle/crop | TODO | Confirm which of the two S&V body wash fronts is the current live asset |
| `santal__vetiver_hand_soap_front.jpg` | S&V | Hand Soap | Packaging photo, front | Listing/PDP hero | TODO | — |
| `santal__vetiver_shapoo_front.jpg` | S&V | Shampoo | Packaging photo, front | Listing/PDP hero | TODO | Filename has the repo-wide "shapoo" typo — see naming convention section |
| `santal__vetiver_shapoo_back.jpg` | S&V | Shampoo | Packaging photo, back | Ingredient panel / A+ Module 6 | TODO | Same typo |
| `santal__vetiver_shapoo_back_1.jpg` | S&V | Shampoo | Packaging photo, back (variant) | Alt angle/crop | TODO | Confirm which back-of-box shot is current |
| `santal__vetiver_shampoo.jpg` | S&V | Shampoo | Packaging photo (untyped) | Unclear — possibly a corrected re-export of the "shapoo" files | TODO | Needs confirmation: is this the replacement for the typo'd files above, or a distinct asset? |
| `FA_SantalVetiverBodyLotion475mLRetail_BottleRenderFINAL_031026.png` | S&V | Body Lotion (475mL retail) | 3D bottle render, final | Listing hero / A+ header | TODO | "FINAL" + date-coded filename — this naming pattern is the one worth standardizing on (see below) |
| `hinoki__bergamot_hand_soap_front.jpg` | H&B | Hand Soap | Packaging photo, front | Listing/PDP hero | TODO | — |
| `hinoki__bergamot_shapoo_front.jpg` | H&B | Shampoo | Packaging photo, front | Listing/PDP hero | TODO | "shapoo" typo |
| `hinoki__bergamot_shapoo_back.jpg` | H&B | Shampoo | Packaging photo, back | Ingredient panel | TODO | "shapoo" typo |
| `hinoki__bergamot.jpg` | H&B | Unclear — generic filename | Unclear | Possibly a brand-deck crop, not a dedicated product asset | TODO | Needs identification before reuse; per prior session notes, H&B body wash specifically lacks a dedicated high-res render (only a low-res deck crop existed) — confirm if this is that same gap |
| `amber__sandalwood_body_wash_front.jpg` | A&S | Body Wash | Packaging photo, front | Listing/PDP hero | TODO | A&S is being discontinued — do not invest further design work here; retain for historical reference only |
| `amber__sandalwood_shapoo_front.jpg` | A&S | Shampoo | Packaging photo, front | Listing/PDP hero | TODO | "shapoo" typo; A&S discontinuation applies |
| `amber__sandalwood_shapoo_back.jpg` | A&S | Shampoo | Packaging photo, back | Ingredient panel | TODO | "shapoo" typo; A&S discontinuation applies |
| `amber__sandalwood_shampoo.jpg` | A&S | Shampoo | Packaging photo (untyped) | Possibly corrected re-export | TODO | Same ambiguity as the S&V untyped shampoo file above; A&S discontinuation applies |
| `FA_AmberSandalwoodBodyLotion475mLRetail_BottleRenderFINAL_031026.png` | A&S | Body Lotion (475mL retail) | 3D bottle render, final | Listing hero | TODO | A&S discontinuation applies |
| `Body_Wash_Category_Tile.png` | Cross-scent | Body Wash (category) | Category tile graphic | Shop-by-category page tile | TODO | — |
| `refill_pouches_1030x1527_rounded.png` | Cross-scent | Refill pouch (category) | Category/product tile, pre-sized for a Webflow card component (rounded corners baked in) | Shop-all or category page tile | TODO | Dimensions in the filename itself — good practice, worth extending to other tiles |
| `body_lotion_1030x1527_rounded.png` | Cross-scent | Body Lotion (category) | Category/product tile, same sizing as above | Shop-all or category page tile | TODO | — |
| `ChatGPT_Image_Jun_16_2026_04_01_10_PM.png` | Unknown | Unknown | AI-generated image, default export filename | Unidentified | TODO | **Needs identification** — default export name gives no context on scent/product/purpose. Rename per convention below once identified, or remove if superseded. |
| `3.png`, `8.png`, `13.png` | Unknown | Unknown | Unidentified | Unidentified | TODO | **Needs identification** — likely deck-export or design-tool numbered exports. Same issue as above; a clear example of why the naming convention below matters. |
| `FNA6062LP.png`, `FNA2002LP.png`, `FNA2062LP.png`, `FNA6052LP.png`, `FNA6002LP.png`, `FNA2052LP.png` | Unknown — likely scent-coded by SKU prefix | Likely 2L Pouch renders ("2LP" suffix matches the 2L refill pouch format) | 3D bottle/pouch render | Likely refill-pouch listing/PDP hero images | TODO | **SKU mapping unconfirmed** — these numeric SKU prefixes (`FNA600`, `FNA602`, `FNA605`, `FNA606`, `FNA200`, `FNA205`, `FNA206`) don't match the SKU pattern already documented elsewhere in this repo (`FNA305`/`FNA306`/`FNA300`). Cross-reference against the SKU List tab in `For_Against_Product_Information.xlsx` (or `data/products.json` once it's the canonical source) before using these in any listing — don't assume the scent/format mapping from the filename alone. |

## Naming convention (proposed — not yet enforced)

The manifest above surfaces real inconsistency: a repeated typo ("shapoo"), several generic/default filenames with zero context (`3.png`, `ChatGPT_Image_...png`), and SKU-coded files that don't match the SKU pattern documented elsewhere. Going forward, new assets should follow:

```
[scent-slug]_[product]_[type]_[status].[ext]
```

- **scent-slug:** `hb` (Hinoki & Bergamot), `sv` (Santal & Vetiver), `as` (Amber & Sandalwood — discontinued, avoid new assets), `xscent` (cross-scent/category, e.g. category tiles)
- **product:** `bodywash`, `shampoo`, `conditioner`, `lotion`, `handsoap`, `deodorant`
- **type:** `front`, `back`, `render`, `lifestyle`, `categorytile`, `icon`
- **status:** `final`, `draft`, `v2` (avoid bare version-less filenames once an asset has been revised)

Example: `sv_bodywash_render_final.png`. The existing `FA_[Scent][Product][Size][Format]_BottleRenderFINAL_[MMDDYY].png` pattern (seen on the two 475mL body lotion renders) is close to this and fine to keep for 3D renders specifically — the goal is consistency, not forcing every asset into one rigid template.

## What this manifest does not cover

- Lifestyle photography gaps flagged in prior sessions (e.g. the Module 2 header image for A+ Content needing a dedicated body-wash-bottle bathroom shot, since existing lifestyle renders were built around a shampoo-box pairing) — tracked in [amazon-a-plus-content.md](../docs/channels/amazon-a-plus-content.md) instead, since that's a production gap, not an asset that exists and needs indexing.
- Design system tokens/colors — out of scope per `AGENTS.md`; see `docs/design-system/` only when a dedicated design-system task calls for it.
