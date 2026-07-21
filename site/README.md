# site/ — Webflow website home in the repo

forandagainstbodycare.com is built in Webflow. **Visual design source of truth stays in Webflow** — do not try to mirror the builder in git. This directory versions the parts Webflow versions poorly and gives agents the context needed to propose safe site/CMS updates.

| File/dir | Purpose |
|---|---|
| `cms-schema.md` | Collection/field documentation — what agent-generated CMS content must conform to |
| `custom-code/` | Head/footer snippets, pixels, embeds — Webflow has no undo for these |
| `cms-backups/` | Scheduled Webflow API snapshots of collection content |
| `seo.md` | Meta conventions, redirect list |
| `infrastructure.md` | DNS/SiteGround/domain notes |

**Related brand context (read, don't reinvent):** [docs/brand/voice.md](../docs/brand/voice.md), [docs/brand/scents/](../docs/brand/scents/), [data/products.json](../data/products.json). Catalog changes update `products.json` first, then CMS.
