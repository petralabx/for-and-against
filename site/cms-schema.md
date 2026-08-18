# Webflow CMS schema

**Last verified:** 2026-08-18 · Site: forandagainstbodycare.com · Site ID: `6a3056618819e8c10dfe5b49`

Pulled from the live Webflow Data API. Do not invent field slugs.

## Collections

### Scents — `6a307e6967da424b16970563` · slug `scents`

Template: `/scents/{slug}` (page ID `6a307e6a67da424b1697059b`)

| Display name | Slug | Type | Required |
|---|---|---|---|
| Name | `name` | PlainText | yes |
| Slug | `slug` | PlainText | yes |
| Descriptor | `descriptor` | PlainText | no |
| Scent Tag | `scent-tag` | PlainText | no |
| Hero Image | `hero-image` | Image | no |
| Top Notes | `top-notes` | PlainText | no |
| Middle Notes | `middle-notes` | PlainText | no |
| Base Notes | `base-notes` | PlainText | no |
| Amazon URL | `amazon-url` | Link | no |
| Accent Color | `accent-color` | PlainText | no |
| Swatch Color | `swatch-color` | Color | no |
| Badge Text | `badge-text` | PlainText | no |
| SEO Title | `seo-title` | PlainText | no |
| SEO Description | `seo-description` | PlainText | no |

Live items: Bergamot & Hinoki (`bergamot-hinoki`), Santal & Vetiver (`santal-vetiver`), Amber & Sandalwood (`amber-sandalwood`).

### Products — `6a309bc46469e785256f3189` · slug `products`

Template: `/products/{slug}` (page ID `6a309bc56469e785256f31a9`). **Item URLs 404 on the live domain as of 2026-08-18** and are not in `sitemap.xml`. Shop cards appear to route to Amazon, not these PDPs.

| Display name | Slug | Type | Required | Notes |
|---|---|---|---|---|
| Name | `name` | PlainText | yes | |
| Slug | `slug` | PlainText | yes | |
| Scent | `scent` | PlainText | no | Display string; live site uses Bergamot & Hinoki |
| Category | `category` | Option | no | Shampoo, Conditioner, Body Wash, Body Lotion, Aluminum Free Deodorant, Refill Pouches |
| Size | `size` | PlainText | no | |
| Price | `price` | Number | no | Do not use for display |
| Price Display | `price-display` | PlainText | no | Canonical display string from `data/products.json` |
| USD | `usd` | PlainText | no | Inconsistent formatting in live items |
| Product Image | `product-image` | Image | no | |
| Amazon URL | `amazon-url` | Link | no | Attribution-tagged when present |
| Featured | `featured` | Switch | no | |
| Prime Eligible | `prime-eligible` | Switch | no | |
| Star Rating | `star-rating` | PlainText | no | |
| Review Count | `review-count` | PlainText | no | |
| Scent Reference | `scent-reference` | Reference → Scents | no | |
| SEO Title | `seo-title` | PlainText | no | |
| SEO Description | `seo-description` | PlainText | no | |

## Critical field rules

- **Price Display is Plain Text**, not Number — this preserves the "$" symbol. Any generated CMS content must supply price as a string exactly matching `price_display` in `data/products.json` (e.g. `"$15.99"`). Never write a bare number.
- Content generated for the CMS must map to these fields — never freeform web copy.
- **SEO Title / SEO Description** must match [site/seo.md](seo.md). Binding those fields to the collection template `<title>` / meta description is a Designer step.

## Sync rule

`data/products.json` is upstream of the CMS. Price or catalog changes: update products.json → then CMS. The scheduled CMS backup (below) doubles as the drift check.
