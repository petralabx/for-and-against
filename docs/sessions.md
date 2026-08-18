# Session log

Append-only handoff log for humans and agents. **Newest entry at the top.**

Read the latest 1–3 entries at the start of every working session (after `git pull` on `main`). Append a short entry in the same PR that lands the work — before merge.

## How to use

```bash
git checkout main && git pull
# skim this file, then branch and work
```

**Entry format** (copy the template):

```markdown
## YYYY-MM-DD — short title

- **Who:** name / agent runtime (e.g. Stephen + Cursor)
- **PR:** #N or n/a
- **Done:** 1–3 bullets of what landed
- **Next:** open follow-ups for the next person
- **Watch:** risks, known issues touched, files that are fragile
```

Keep entries short. Deep write-ups belong in `docs/learnings.md` (experiments/issues) or a dedicated docs page — not here.

---

## 2026-08-18 — SEO metadata + breadcrumbs for Webflow

- **Who:** Stephen + Cursor
- **PR:** pending (`seo/metadata-and-breadcrumbs`)
- **Done:** Shipped unique titles/descriptions, Organization/WebPage JSON-LD, breadcrumbs, Scents template SEO (Designer-bound), image alts, one H1 per key page, and Shop By Scent → `/#find-your-scent`. Repo source of truth: [site/seo.md](../site/seo.md) (approved copy + remaining work), [site/cms-schema.md](../site/cms-schema.md), [site/custom-code/](../site/custom-code/). Live on apex + www as of 17:26 UTC.
- **Next (pick up from [site/seo.md](../site/seo.md) Remaining work):** (1) Webflow default domain so www 301s to apex. (2) Amazon Attribution URLs on Shop/scent Buy buttons (`href="#"` today) — Stephen owns this; campaign `For & Against Website`. (3) Bind scent fragrance-story — Bergamot/Santal pages still show Amber copy. (4) Footer legal links still point at `for-against.webflow.io`. (5) Decide whether `/products/{slug}` should exist (404, not in sitemap). Search Console Domain property + apex sitemap already submitted.
- **Watch:** Never paste head/footer without keeping GA4 `G-TSLV0FDSBD`. Do not check “Get URL from Scents” on the three static scent tabs. Amber & Sandalwood is still on-site but flagged for discontinuation. Comforting Body Lotion | Bergamot & Hinoki was published from draft so its alt could go live.

## 2026-08-11 — visual asset manifest and A+ Content template added

- **Who:** Greg + Claude (claude.ai, GitHub MCP connector, Webflow MCP connector)
- **PR:** n/a (see `site/assets-manifest-and-aplus-content-template` branch)
- **Done:** Added `site/assets-manifest.md` — an index of known image assets (filename → scent/product/type → likely purpose → Webflow URL) rather than committing the image binaries themselves. Added `docs/channels/amazon-a-plus-content.md` — the 7-module A+ Content template (specs, copy patterns, known gaps) generalized from the Santal & Vetiver body wash build so it can be reused for Hinoki & Bergamot. Cross-linked both from `site/README.md`. **Follow-up same session:** connected the Webflow MCP connector and pulled the live Assets API (38 assets) to fill in real URLs — confirmed only 4 manifest entries actually exist on Webflow (the 3 category tiles + 1 hero image); every scent-specific packaging photo, 3D bottle render, and `FNA*2LP` render is confirmed **not** on Webflow, consistent with the site having no product detail pages. Also surfaced that duplicate-looking category tile assets exist from two upload batches (Jun 16 vs Jun 30) with no indication which is live, and that the homepage hero currently runs on AI-generated imagery.
- **Next:** Several filenames still need identification (`3.png`, `8.png`, `13.png` — confirmed not on Webflow either, so likely knowledge-base-only exports) and the `FNA*2LP` SKU prefixes still don't match the documented `FNA305`/`FNA306`/`FNA300` pattern — cross-reference against `For_Against_Product_Information.xlsx` before using. A repeated "shapoo" typo across several shampoo asset filenames should be fixed at the source if those get renamed. Someone with Webflow Designer access should confirm which of the duplicate category tile generations is actually live on each page.
- **Watch:** No image binaries were committed to the repo, intentionally — this manifest is an index only, per the reasoning in `site/assets-manifest.md`. Don't let a future session start committing raw images here.

## 2026-08-11 — Amazon Attribution structure documented

- **Who:** Greg + Claude (claude.ai, GitHub MCP connector)
- **PR:** n/a (see `docs/amazon-attribution-structure` branch)
- **Done:** Added `docs/channels/amazon-attribution.md` — campaign/ad group/tag hierarchy, current ad group lists for both the `For & Against Website` and `For & Against — Google Ads` campaigns, naming convention, campaign-level product pool rule, URL handling rules, and platform quirks (no native delete, wizard closing after campaign creation, per-SKU eligibility gaps). Cross-linked from `docs/channels/amazon.md`.
- **Next:** `docs/channels/paid-ads.md` (PR #13, not yet merged) has a placeholder reference to this doc that should be swapped for a real link once both PRs land — whichever merges second should update the link. A tag-name → tracking-URL → Google Ads-keyword mapping doc is still unbuilt; recommended once the Google Ads ad group count grows.
- **Watch:** This branch was cut from `main` before PR #13 merged, so it doesn't carry that PR's `paid-ads.md` changes — no conflict expected since different files were touched, but worth double-checking on merge.

## 2026-08-05 — 2026-07 keyword snapshots imported

- **Who:** Greg + Claude (claude.ai, GitHub MCP connector)
- **PR:** n/a (see `add-2026-07-keyword-snapshots` branch)
- **Done:** Imported the deferred 2026-07 Jungle Scout keyword export set into `data/keywords/2026-07/` — body wash, shampoo, conditioner, and lotion across retail/refill-pouch/gallon-case formats (10 files). Each trimmed to top 140 keywords by search volume rather than the full raw export; see `data/keywords/2026-07/README.md` for methodology.
- **Next:** `docs/channels/paid-ads.md` still needs the Phase 1/2 Google Ads plan (budget split, campaign structure) written in using this data. Deodorant and Amber & Sandalwood keyword pulls are not included in this snapshot (A&S is being discontinued).
- **Watch:** These are trimmed snapshots, not full raw exports — if deeper long-tail keyword analysis is needed later, re-pull from Jungle Scout rather than assuming this file is exhaustive.

## 2026-08-05 — session log convention added

- **Who:** Stephen + Cursor
- **PR:** #10
- **Done:** Added this file; wired start/end-of-session discipline into `AGENTS.md`
- **Next:** Colleagues pull `main`, read latest entries before starting; append an entry with each meaningful PR
- **Watch:** Soft MC compliance — operator PRs do not need `MC-Checkout`
