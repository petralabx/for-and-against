# Custom code (head/footer/embeds)

Webflow has no version control for site-wide custom code. Every snippet that goes into Site Settings → Custom Code or page-level embeds gets committed here first, so there's always an undo.

Convention: one file per snippet, named by function — e.g. `head-ga4.html`, `head-meta-pixel.html`, `footer-schema-org.html`. Include a comment header: what it does, where it's installed (site-wide vs page), date installed.

**TODO:** export current live head/footer code from Webflow Site Settings and commit as the baseline.
