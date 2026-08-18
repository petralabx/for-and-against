# Website SEO conventions

**Last verified:** 2026-08-18 · Live site: [forandagainstbodycare.com](https://forandagainstbodycare.com) · Webflow site ID: `6a3056618819e8c10dfe5b49`

Approved titles, descriptions, Open Graph, JSON-LD, and breadcrumb labels for the Webflow site. Push changes to Webflow from this file — do not invent live metadata.

## Rules

- **Title:** `[Page] | For & Against`, ≤60 characters. Homepage is brand + category positioning.
- **Description:** 120–155 characters, voice per [docs/brand/voice.md](../docs/brand/voice.md). No competitor names. No "All Natural" / "Non-Toxic". Approved claims only ([docs/compliance/claims.md](../docs/compliance/claims.md)).
- **Scent naming on-site:** match live Webflow/Amazon — **Bergamot & Hinoki** (not "Hinoki & Bergamot").
- **Canonical host:** sitemap uses apex `https://forandagainstbodycare.com` (no www). JSON-LD and breadcrumbs use the same host.
- **Keyword source:** `data/keywords/2026-07/` (snapshot dated 2026-03-19). Transferable Google intent: luxury body wash, body lotion, sulfate-free shampoo, aluminum-free deodorant, refill pouches. Do not put competitor brand names in metadata even when they dominate the Amazon keyword file.
- **Site job:** send qualified traffic to Amazon (Attribution-tagged links). Metadata can name Amazon; it should not read like a listing title dump.
- **Redirects:** Webflow redirect panel is live; this file mirrors it. _(none recorded)_

## Live audit (2026-08-18, before this pass)

| Gap | Evidence |
|---|---|
| No meta descriptions on any URL | Live `<head>` has no `name="description"` |
| Duplicate titles | Home + all `/scents/*` render `<title>For & Against` |
| Thin titles | `/shop`, `/best-sellers`, `/contact`, legal pages use the nav label only |
| No JSON-LD | Zero `application/ld+json` on sampled pages |
| No breadcrumbs | No visible trail; no `BreadcrumbList` |
| Product CMS routes 404 | `/products/{slug}` 404s on custom domain and `for-against.webflow.io`; not in sitemap |
| Sitemap is static + scents only | 10 URLs; no product items |
| `site:forandagainstbodycare.com` | No indexed results sampled on 2026-08-18 (new site / thin metadata) |

## Static pages

| Path | Page ID | Title (chars) | Meta description |
|---|---|---|---|
| `/` | `6a3056628819e8c10dfe5bf9` | For & Against \| Luxury Body Care Without the Markup (51) | Luxury body wash, shampoo, lotion, and deodorant with a scent that lasts. Sulfate free, vegan, filled in our own facility. Shop on Amazon. |
| `/shop` | `6a32c0c9788a6f5667889686` | Shop Luxury Body Care \| For & Against (37) | Shop body wash, shampoo, lotion, and aluminum-free deodorant. Long-lasting scents and refill pouches, without the markup. Available on Amazon. |
| `/best-sellers` | `6a4e8b5f6915fb4b69635cc1` | Best Sellers \| For & Against (28) | The most-loved For & Against body wash, shampoo, lotion, and deodorant. Luxury scent that lasts, without the markup. Shop best sellers on Amazon. |
| `/contact` | `6a32c95278077654ac5c8163` | Contact \| For & Against (23) | Questions about For & Against body care, orders, or wholesale? Get in touch. Products ship via Amazon US, with Prime delivery where eligible. |
| `/returns-shipping` | `6a32d559b4634baa2bf98370` | Returns & Shipping \| For & Against (34) | Shipping and returns for For & Against body care. Orders fulfill through Amazon US, including Prime delivery where eligible. |
| `/privacy-policy` | `6a32d44f79ba4b3a5c0a337b` | Privacy Policy \| For & Against (30) | How For & Against collects, uses, and protects personal information on forandagainstbodycare.com. |
| `/terms-conditions` | `6a32c909cf446cb0ca21eef9` | Terms & Conditions \| For & Against (34) | Terms of use for the For & Against website. Products are sold and fulfilled via Amazon US. |

Skip: draft `Untitled` page (`/static-template-slug-1781715979104`). Do not index it.

Open Graph: copy title + description from SEO. Default `og:image` is the site logo:

`https://cdn.prod.website-files.com/6a3056618819e8c10dfe5b49/6a3059f93cf2f21f3a4f8846_F%26A_Logos-01%20(1).png`

## CMS — Scents (`/scents/{slug}`)

Template page ID: `6a307e6a67da424b1697059b`. Unique titles require the template SEO fields to be bound to CMS `seo-title` / `seo-description` in Webflow Designer (API cannot bind template SEO to a field). Values below are stored on each item.

| Slug | Item ID | Title | Meta description |
|---|---|---|---|
| `bergamot-hinoki` | `6a3080287c320eeb8c95f663` | Bergamot & Hinoki \| For & Against | A citrus-woody fragrance that lasts. Italian bergamot and grapefruit open bright, then hinoki and amber. Shop Bergamot & Hinoki on Amazon. |
| `santal-vetiver` | `6a30808ab9e86c55770d0daa` | Santal & Vetiver \| For & Against | A warm, smoky fragrance that lasts. Sandalwood and soft spice settle into vetiver and cedar. Shop Santal & Vetiver body care on Amazon. |
| `amber-sandalwood` | `6a3080de23de857e17af30ea` | Amber & Sandalwood \| For & Against | A creamy, woody fragrance that lasts. Warm sandalwood and vanilla settle into musk and amber. Shop Amber & Sandalwood on Amazon. |

Amber & Sandalwood remains on the live site; it is being discontinued operationally — do not add new A&S landing pages.

## CMS — Products (`/products/{slug}`)

Template page ID: `6a309bc56469e785256f31a9`. **Item URLs currently 404** and are omitted from the sitemap. SEO fields are still filled so they are ready if the template is published as public PDPs. Until then they are not a SERP surface.

Pattern: `{Scent} {Format} | For & Against`.

| Slug | Status | Title |
|---|---|---|
| `no-nonsense-body-wash` | live | Santal & Vetiver Body Wash \| For & Against |
| `no-nonsense-body-wash-2` | live | Amber & Sandalwood Body Wash \| For & Against |
| `no-nonsense-body-wash-bergamot-hinoki` | live | Bergamot & Hinoki Body Wash \| For & Against |
| `body-wash-refill-pouch-santal-vetiver` | live | Santal & Vetiver Wash Refill \| For & Against |
| `body-wash-refill-pouch-bergamot-hinoki` | live | Bergamot & Hinoki Wash Refill \| For & Against |
| `stress-free-shampoo-santal-vetiver` | live | Santal & Vetiver Shampoo \| For & Against |
| `stress-free-shampoo-bergamot-hinoki` | live | Bergamot & Hinoki Shampoo \| For & Against |
| `stress-free-shampoo-amber-sandalwood` | live | Amber & Sandalwood Shampoo \| For & Against |
| `comforting-body-lotion-santal-vetiver` | live | Santal & Vetiver Body Lotion \| For & Against |
| `comforting-body-lotion-amber-sandalwood` | live | Amber & Sandalwood Body Lotion \| For & Against |
| `comforting-body-lotion` | draft | Bergamot & Hinoki Body Lotion \| For & Against |
| `body-lotion-refill-pouch-santal-vetiver` | live | Santal & Vetiver Lotion Refill \| For & Against |
| `body-lotion-refill-pouch-bergamot-hinoki` | live | Bergamot & Hinoki Lotion Refill \| For & Against |
| `aluminum-free-deodorant` | live | Santal & Vetiver Deodorant \| For & Against |
| `aluminum-free-deodorant-bergamot-hinoki` | live | Bergamot & Hinoki Deodorant \| For & Against |
| `aluminum-free-deodorant-amber-sandalwood` | draft | Amber & Sandalwood Deodorant \| For & Against |

Product descriptions use the matching scent story plus format (wash / shampoo / lotion / deodorant / refill), ≤155 characters. Stored on the item `seo-description` field.

## Breadcrumbs

Visible trail (not on Home) + matching `BreadcrumbList` JSON-LD.

| URL | Trail |
|---|---|
| `/` | Home (JSON-LD Organization + WebSite only; no visible crumbs) |
| `/shop` | Home > Shop |
| `/best-sellers` | Home > Best Sellers |
| `/contact` | Home > Contact |
| `/returns-shipping` | Home > Returns & Shipping |
| `/privacy-policy` | Home > Privacy Policy |
| `/terms-conditions` | Home > Terms & Conditions |
| `/scents/{slug}` | Home > {Scent name} |
| `/products/{slug}` | Home > Shop > {Product title} |

There is no `/scents` index URL — do not invent a parent crumb that 404s. Nav "Shop By Scent" points at `/#find-your-scent` on the homepage. Scent tabs on the Scents template stay hardcoded to each `/scents/{slug}` — do not bind those tab URLs to the current CMS item.

Implementation: site-wide custom code in [custom-code/](custom-code/) (CSS in head, injector in footer). Static pages also get JSON-LD via Webflow page settings so Google does not depend on JS for those URLs.

## JSON-LD types

- Home: `Organization` + `WebSite` + `WebPage`
- Shop, Best Sellers: `CollectionPage` + `BreadcrumbList`
- Contact: `ContactPage` + `BreadcrumbList`
- Legal / returns: `WebPage` + `BreadcrumbList`
- Scents (via footer script until template SEO is bound): `WebPage` + `BreadcrumbList`

Do not add `Product` schema until `/products/{slug}` returns 200 with price/availability that match Amazon. Do not add `SearchAction` — the site has no on-site search.

## Designer follow-up (required for unique scent `<title>` tags)

API page SEO on a collection template is a single fallback string. To emit unique titles in HTML:

1. Collection Template (Scents) → Page Settings → SEO title → Add field → `SEO Title`
2. Same for SEO description → `SEO Description`
3. Repeat on the Products template if PDPs are turned on

Until that bind, scent pages keep the site-name title even when CMS fields are populated.
