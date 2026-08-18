# Custom code (head/footer/embeds)

Webflow has no version control for site-wide custom code. Every snippet that goes into Site Settings → Custom Code or page-level embeds gets committed here first, so there's always an undo.

Convention: one file per snippet, named by function — e.g. `head-ga4.html`, `head-meta-pixel.html`, `footer-schema-org.html`. Include a comment header: what it does, where it's installed (site-wide vs page), date installed.

Live site-wide blocks (export = source of truth to paste back):

| File | Where | What |
|---|---|---|
| `head-site.html` | Site Settings → Custom Code → Head | Scrollbar/scent-tab CSS, Tabler icons, GA4 `G-TSLV0FDSBD`, breadcrumb CSS |
| `footer-site.html` | Site Settings → Custom Code → Footer | Scent-tab current-page underline, visible breadcrumbs, BreadcrumbList JSON-LD fallback |

GA4 and the scent-tab scripts were already live (exported 2026-08-18). Breadcrumb CSS/JS were added in the same blocks — do not replace head/footer with a fragment or GA4 will disappear.
