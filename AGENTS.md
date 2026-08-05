# AGENTS.md — For & Against brand repo

Entry point for any agent or collaborator working in this repo. Read this before generating anything.

## What this repo is

Versioned source of truth for **For & Against** brand knowledge, product data, channel playbooks, approved copy, and Webflow site ops (forandagainstbodycare.com). Amazon Store + Webflow are the live surfaces this repo supports.

- `docs/` behaves like a wiki (small dense pages, revised in place, cross-linked — git history is the archive).
- `data/` and `copy/` behave like archives (append; don't rewrite history casually).
- `site/` versions what Webflow versions poorly (CMS schema, custom code, SEO, infra). Design source of truth for the live site stays in Webflow; this repo holds agent-usable context for safe updates.

`docs/design-system/` is **out of scope for brand-ops work** — leave it alone unless a dedicated design-system task says otherwise. For Webflow/visual context, prefer `site/`, `docs/brand/`, and existing `--fa-` tokens only when needed.

## Source-of-truth rules

- `data/products.json` is the **canonical product record** (SKUs, scents, formats, prices, ASINs). Webflow CMS, Amazon listings, and generated copy sync **from** it. If surfaces disagree, verify against live Amazon and fix `products.json` first.
- Every data file carries a `last_verified` date. Flag staleness rather than silently using old numbers.
- Keyword exports in `data/keywords/` are dated snapshots — never treat them as current beyond their folder date.

## Brand voice (summary — full guide in docs/brand/voice.md)

Premium body care positioned as the accessible-luxury alternative to Salt & Stone / Ouai / Necessaire. Voice: elevated, minimal, confident. Not clinical, not cutesy, not over-explained. Audience: health-conscious 25–45, urban, cares about wellness and bathroom aesthetic.

## Hard guardrails

1. **Never put COGS, fees, margins, or any internal financial figures in this repo** or in content generated from it. Retail prices are fine.
2. **Dupe/competitor-comparison language is channel-restricted** — see `docs/compliance/claims.md`. Never in Amazon listing copy. Allowed (with care) in TikTok creator briefs, affiliate materials, and organic social.
3. **Claims compliance:** only claims listed as approved in `docs/compliance/claims.md`. No drug claims (treats/cures/heals). Avoid "All Natural" / "Non-Toxic" in new copy.
4. **API keys and secrets never enter this repo** — GitHub Actions Secrets only.
5. Do not invent Webflow CMS field names — use `site/cms-schema.md` and fetch the live schema when incomplete.
6. Import MC task discipline: every agent PR stamps `MC-Checkout: <task-id>`.

## Workflow discipline

- **Start of session:** `git checkout main && git pull`, then read the latest entries in `docs/sessions.md` (and `docs/learnings.md` before touching Amazon copy).
- Copy changes to live surfaces (Amazon listings, Webflow CMS) go through PR review — never direct to `main`.
- New approved copy lands in `copy/`; failed experiments get logged in `docs/learnings.md`.
- Catalog/price changes: update `data/products.json` first, then CMS / listing files.
- **End of session / before merge:** append a short entry to `docs/sessions.md` in the same PR (who, PR, done, next, watch).

## Repo map

| Path | What lives there |
|---|---|
| `docs/brand/` | Voice guide, scent pages |
| `docs/competitors/` | Competitor pages + dupe map |
| `docs/channels/` | Amazon, email, paid ads, TikTok playbooks |
| `docs/compliance/` | Approved/banned claims, US + CA notes |
| `docs/wholesale/` | Distributor/retail strategy, bulk SKUs |
| `docs/analytics/` | KPI definitions |
| `docs/learnings.md` | Experiment log + known-issue log |
| `docs/sessions.md` | Append-only session handoff (read first, write last) |
| `docs/design-system/` | Separate — do not modify for brand-ops tasks |
| `site/` | Webflow: CMS schema, custom code, SEO, infra, CMS backups |
| `data/` | `products.json` (canonical), pricing watch list, keyword snapshots |
| `copy/` | Approved copy only — Amazon listings, email, ads |
