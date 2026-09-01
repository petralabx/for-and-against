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

## Live status (2026-08-18, after publish 17:26 UTC)

Shipped: unique titles + meta descriptions on static pages and `/scents/*`, Open Graph copy-from-SEO, Organization/WebPage JSON-LD, visible breadcrumbs + BreadcrumbList, image alts, heading hierarchy below, Shop By Scent → `/#find-your-scent`.

Search Console: Domain property `forandagainstbodycare.com` + sitemap `https://forandagainstbodycare.com/sitemap.xml` already submitted. No further GSC setup needed this pass.

| Still open | Evidence |
|---|---|
| www and apex both 200 | No 301. Set Webflow **Publishing → Default domain** to apex `forandagainstbodycare.com` |
| Shop / scent “Buy on Amazon” is `href="#"` | Home/Best Sellers have some Attribution links; Shop and scent pages do not. Stephen owns pasting tags — campaign `For & Against Website` ([docs/channels/amazon-attribution.md](../docs/channels/amazon-attribution.md)) |
| Scent fragrance story is static Amber copy | Bergamot page still says “Sandalwood is a creamy, woody fragrance…”. No CMS field — add one or bind existing copy |
| Footer legal URLs | Point at `for-against.webflow.io`, not the custom domain. Footer About/Contact are `#` |
| Product CMS routes 404 | `/products/{slug}` 404s; not in sitemap. SEO fields are filled for if PDPs go live |
| Products template SEO unbound | Bind only if those URLs become public |
| Rel canonical missing | Webflow page canonical not set; host duplicate until default-domain 301 exists |

## Remaining work (next session)

1. **Default domain** — Webflow Site settings → Publishing → set default to `forandagainstbodycare.com` so www 301s to apex. Matches sitemap + JSON-LD host.
2. **Amazon Attribution** — replace `#` Buy buttons on Shop and each `/scents/{slug}` with placement-specific tags (not one sitewide tag). Do not invent tags here.
3. **Fragrance story** — add a Scents CMS PlainText/RichText field and bind the story `<p class="paragraph-2">` on the Scents template. Copy from [docs/brand/scents/](../docs/brand/scents/). Live notes fields are already CMS-bound.
4. **Footer** — point legal links at the custom domain; wire About/Contact.
5. **PDP decision** — keep `/products/{slug}` unpublished, or publish the Products template and bind `seo-title` / `seo-description` the same way Scents was bound.
6. **Canonical tags** — after the 301, set Webflow canonicals to apex if the designer still emits none.

## Heading hierarchy (live)

| Page | H1 | Notes |
|---|---|---|
| `/` | Personal Care That Works With Your Body | Best Sellers + Find Your Scent are H2. Find Your Scent heading has `id="find-your-scent"` |
| `/shop` | Shop All Products | |
| `/best-sellers` | Best Sellers | |
| `/scents/{slug}` | Scent name (`Heading 12`) | Was H4. The Explore-Scent card title is not the page H1 |

Scent tabs (`scent-tab-link`) are **hardcoded** to `/scents/amber-sandalwood`, `/scents/bergamot-hinoki`, `/scents/santal-vetiver`. Do **not** check “Get URL from Scents” on those three — that would make every tab point at the current CMS item.

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

Template page ID: `6a307e6a67da424b1697059b`. **SEO title/description are bound in Designer** (2026-08-18) — live `<title>` is unique per scent (e.g. `Bergamot & Hinoki | For & Against`). Values below are stored on each item. API cannot bind template SEO; if titles regress, re-bind Page Settings → SEO → Add field.

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
- Scents: `WebPage` + `BreadcrumbList` (page SEO bound; footer script still injects BreadcrumbList if the page HTML has none)

Do not add `Product` schema until `/products/{slug}` returns 200 with price/availability that match Amazon. Do not add `SearchAction` — the site has no on-site search.

## Designer notes

Scents template SEO bind is **done**. Products template: skip until PDPs are public.

If unique scent titles disappear: Collection Template (Scents) → Page Settings → SEO title / description → Add field → `SEO Title` / `SEO Description`.
