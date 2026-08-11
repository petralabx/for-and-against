# Visual asset manifest

**Last verified:** 2026-08-11 · Links: [cms-schema](cms-schema.md) · [products.json](../data/products.json) · [design tokens](../docs/design-system/) (colors only — do not edit that dir)

**Image files themselves are not committed to this repo.** They live in Webflow's Assets panel (site-rendering copies) and in this project's Claude knowledge base (working copies for content generation). This file is the index that lets a person or agent find the right asset without duplicating binaries into git history. Update this table whenever assets are added, replaced, or retired — don't let it drift from what's actually live.

**Webflow connector now active** — the table below reflects a real pull from the site's Assets API (38 assets total, site ID `6a3056618819e8c10dfe5b49`). Rows below are marked either with a live URL (confirmed present on Webflow) or **"Not found on Webflow"** (confirmed absent, not just unchecked).

## Key finding: most product photography lives on Amazon/knowledge-base only, not Webflow

None of the scent-specific packaging photos (`santal__vetiver_*`, `hinoki__bergamot_*`, `amber__sandalwood_*`) or the two `FA_*BottleRenderFINAL*` 3D renders, or the six `FNA*2LP` renders, exist in the Webflow Assets library. This isn't a gap in this manifest — it's consistent with the site having no on-site product detail pages (per `AGENTS.md`, all product traffic routes to Amazon). Those assets are Amazon-listing/A+-Content material and correctly live in the project knowledge base and/or Amazon's own image library, not Webflow. Only cross-scent category tiles and hero/banner imagery live in Webflow's Assets panel.

## Manifest

| Filename (as known) | Scent | Product | Type | Likely purpose | Webflow Asset URL | Status / notes |
|---|---|---|---|---|---|---|
| `santal__vetiver_body_wash_front.jpg` | S&V | Body Wash | Packaging photo, front | Amazon listing/A+ hero | **Not found on Webflow** | Confirmed Amazon/knowledge-base-only asset — see key finding above |
| `santal__vetiver_body_wash_front_1.jpg` | S&V | Body Wash | Packaging photo, front (variant) | Alt angle/crop | **Not found on Webflow** | Same — also still need to confirm which of the two variants is current for Amazon use |
| `santal__vetiver_hand_soap_front.jpg` | S&V | Hand Soap | Packaging photo, front | Amazon listing hero | **Not found on Webflow** | — |
| `santal__vetiver_shapoo_front.jpg` | S&V | Shampoo | Packaging photo, front | Amazon listing hero | **Not found on Webflow** | Filename has the repo-wide "shapoo" typo — see naming convention section |
| `santal__vetiver_shapoo_back.jpg` | S&V | Shampoo | Packaging photo, back | Ingredient panel / A+ Module 6 | **Not found on Webflow** | Same typo |
| `santal__vetiver_shapoo_back_1.jpg` | S&V | Shampoo | Packaging photo, back (variant) | Alt angle/crop | **Not found on Webflow** | Confirm which back-of-box shot is current |
| `santal__vetiver_shampoo.jpg` | S&V | Shampoo | Packaging photo (untyped) | Unclear — possibly a corrected re-export of the "shapoo" files | **Not found on Webflow** | Needs confirmation: is this the replacement for the typo'd files above, or a distinct asset? |
| `FA_SantalVetiverBodyLotion475mLRetail_BottleRenderFINAL_031026.png` | S&V | Body Lotion (475mL retail) | 3D bottle render, final | Amazon listing hero / A+ header | **Not found on Webflow** | "FINAL" + date-coded filename — this naming pattern is the one worth standardizing on (see below) |
| `hinoki__bergamot_hand_soap_front.jpg` | H&B | Hand Soap | Packaging photo, front | Amazon listing hero | **Not found on Webflow** | — |
| `hinoki__bergamot_shapoo_front.jpg` | H&B | Shampoo | Packaging photo, front | Amazon listing hero | **Not found on Webflow** | "shapoo" typo |
| `hinoki__bergamot_shapoo_back.jpg` | H&B | Shampoo | Packaging photo, back | Ingredient panel | **Not found on Webflow** | "shapoo" typo |
| `hinoki__bergamot.jpg` | H&B | Unclear — generic filename | Unclear | Possibly a brand-deck crop, not a dedicated product asset | **Not found on Webflow** | Needs identification before reuse; per prior session notes, H&B body wash specifically lacks a dedicated high-res render (only a low-res deck crop existed) — confirm if this is that same gap |
| `amber__sandalwood_body_wash_front.jpg` | A&S | Body Wash | Packaging photo, front | Amazon listing hero | **Not found on Webflow** | A&S is being discontinued — do not invest further design work here; retain for historical reference only |
| `amber__sandalwood_shapoo_front.jpg` | A&S | Shampoo | Packaging photo, front | Amazon listing hero | **Not found on Webflow** | "shapoo" typo; A&S discontinuation applies |
| `amber__sandalwood_shapoo_back.jpg` | A&S | Shampoo | Packaging photo, back | Ingredient panel | **Not found on Webflow** | "shapoo" typo; A&S discontinuation applies |
| `amber__sandalwood_shampoo.jpg` | A&S | Shampoo | Packaging photo (untyped) | Possibly corrected re-export | **Not found on Webflow** | Same ambiguity as the S&V untyped shampoo file above; A&S discontinuation applies |
| `FA_AmberSandalwoodBodyLotion475mLRetail_BottleRenderFINAL_031026.png` | A&S | Body Lotion (475mL retail) | 3D bottle render, final | Amazon listing hero | **Not found on Webflow** | A&S discontinuation applies |
| `Body_Wash_Category_Tile.png` | Cross-scent | Body Wash (category) | Category tile graphic | Shop-by-category page tile | [Live asset](https://s3.amazonaws.com/webflow-prod-assets/6a3056618819e8c10dfe5b49/6a31a6f77c5597699c24bfca_Body%20Wash%20Category%20Tile.png) | **Confirmed on Webflow.** Uploaded 2026-06-16. Note: a newer "Body Wash Tile Category Final.png" (uploaded 2026-06-30) also exists in the library — confirm with whoever manages the site which one is actually live on the page before treating either as current |
| `refill_pouches_1030x1527_rounded.png` | Cross-scent | Refill pouch (category) | Category/product tile, pre-sized for a Webflow card component (rounded corners baked in) | Shop-all or category page tile | [Live asset](https://s3.amazonaws.com/webflow-prod-assets/6a3056618819e8c10dfe5b49/6a31ab4ff5391d08407ae5d9_refill_pouches_1030x1527_rounded.png) | **Confirmed on Webflow.** Uploaded 2026-06-16. A separate "refill pouch category.png" (2026-06-30) also exists — same "which is live" caveat as above |
| `body_lotion_1030x1527_rounded.png` | Cross-scent | Body Lotion (category) | Category/product tile, same sizing as above | Shop-all or category page tile | [Live asset](https://s3.amazonaws.com/webflow-prod-assets/6a3056618819e8c10dfe5b49/6a31ab4e5897cdbf997eb5e2_body_lotion_1030x1527_rounded.png) | **Confirmed on Webflow.** Uploaded 2026-06-16. A separate "Lotion Category.png" (2026-06-30) also exists — same caveat |
| `ChatGPT_Image_Jun_16_2026_04_01_10_PM.png` | Cross-scent | N/A — hero banner | AI-generated image, default export filename | Homepage hero image | [Live asset](https://s3.amazonaws.com/webflow-prod-assets/6a3056618819e8c10dfe5b49/6a31ab93b6bcb0188bf7bcef_ChatGPT%20Image%20Jun%2016%2C%202026%2C%2004_01_10%20PM.png) | **Confirmed on Webflow.** Several other AI-generated "ChatGPT Image ..." files exist in the same library (Jun 16, Jun 30, Jul 6 uploads) — worth knowing that hero/banner imagery currently runs on AI-generated art rather than real product/lifestyle photography. Still worth renaming per convention below. |
| `3.png`, `8.png`, `13.png` | Unknown | Unknown | Unidentified | Unidentified | **Not found on Webflow** | **Needs identification** — not in the Webflow library either, so these likely live only in the project knowledge base as raw deck-export or design-tool numbered exports. Same issue as above; a clear example of why the naming convention below matters. |
| `FNA6062LP.png`, `FNA2002LP.png`, `FNA2062LP.png`, `FNA6052LP.png`, `FNA6002LP.png`, `FNA2052LP.png` | Unknown — likely scent-coded by SKU prefix | Likely 2L Pouch renders ("2LP" suffix matches the 2L refill pouch format) | 3D bottle/pouch render | Likely Amazon listing hero images for refill pouches | **Not found on Webflow** | **SKU mapping still unconfirmed** — consistent with these being Amazon-only assets (not needed on Webflow, since there are no PDPs), but the numeric SKU prefixes (`FNA600`, `FNA602`, `FNA605`, `FNA606`, `FNA200`, `FNA205`, `FNA206`) still don't match the `FNA305`/`FNA306`/`FNA300` pattern confirmed live elsewhere (see below) — cross-reference against `For_Against_Product_Information.xlsx` before using these in any listing. |

**Confirms the existing SKU pattern:** the Webflow library contains `FNA305-500 - Bergamot & Hinoki - Scent Story Infographic.jpg` — a real, live asset using the `FNA305` prefix already documented elsewhere in this repo for Hinoki & Bergamot. This reinforces that the `FNA6062LP`-style filenames above are a *different* numbering scheme and shouldn't be assumed to map onto scents by guesswork.

## Naming convention (proposed — not yet enforced)

The manifest above surfaces real inconsistency: a repeated typo ("shapoo"), several generic/default filenames with zero context (`3.png`, `ChatGPT_Image_...png`), duplicate-looking category tile generations sitting side by side in Webflow with no indication which is live, and SKU-coded files that don't match the SKU pattern documented elsewhere. Going forward, new assets should follow:

```
[scent-slug]_[product]_[type]_[status].[ext]
```

- **scent-slug:** `hb` (Hinoki & Bergamot), `sv` (Santal & Vetiver), `as` (Amber & Sandalwood — discontinued, avoid new assets), `xscent` (cross-scent/category, e.g. category tiles)
- **product:** `bodywash`, `shampoo`, `conditioner`, `lotion`, `handsoap`, `deodorant`
- **type:** `front`, `back`, `render`, `lifestyle`, `categorytile`, `icon`
- **status:** `final`, `draft`, `v2` (avoid bare version-less filenames once an asset has been revised — the duplicate category tiles above are exactly the failure mode this prevents)

Example: `sv_bodywash_render_final.png`. The existing `FA_[Scent][Product][Size][Format]_BottleRenderFINAL_[MMDDYY].png` pattern (seen on the two 475mL body lotion renders) is close to this and fine to keep for 3D renders specifically — the goal is consistency, not forcing every asset into one rigid template.

## What this manifest does not cover

- Lifestyle photography gaps flagged in prior sessions (e.g. the Module 2 header image for A+ Content needing a dedicated body-wash-bottle bathroom shot, since existing lifestyle renders were built around a shampoo-box pairing) — tracked in [amazon-a-plus-content.md](../docs/channels/amazon-a-plus-content.md) instead, since that's a production gap, not an asset that exists and needs indexing.
- Design system tokens/colors — out of scope per `AGENTS.md`; see `docs/design-system/` only when a dedicated design-system task calls for it.
- Custom fonts (Inter Regular/Bold, Fortika Regular) are uploaded as Webflow assets too, but those are covered by the brand visual-identity guide, not this manifest.
