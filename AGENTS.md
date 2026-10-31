# AGENTS.md — For & Against brand repo

Entry point for any agent or collaborator working in this repo. Read this before generating anything.

## Mission Control handshake (Claude Code and every agent)

<!-- plx-mc:handshake:start -->
Generated from PLX_MC [`config/governance-contract.yaml`](https://github.com/petralabx/PLX_MC/blob/main/config/governance-contract.yaml) by `scripts/generate-governance-surfaces.py --fleet`. Do not edit this block: change the contract and regenerate.

Before the first edit or any PR on `petralabx/for-and-against`:

1. Search projects, buckets and TASK-* first (mc_search_tasks: branch, title, runId). Create a task only on a real miss, with mc_create_task in BKT-INFRA. Never create a task to escape incomplete evidence on a live checkout.
2. Check out with mc_checkout_task, passing repo `petralabx/for-and-against`, your own `runtime/lane` as agent, and your branch. Confirm the returned taskId and that actor.repo is `petralabx/for-and-against`. Copy prBodyLine exactly.
3. Never invent a dsp_* id, never write `MC-Checkout: pending`, and never push with --no-verify. Without the MC tools, check out over HTTP: `COMPLIANCE_CAPTURE=1 MC_REPO=petralabx/for-and-against MC_TASK_ID=TASK-N node scripts/compliance-checkout.mjs`. If that fails too, stop and ask CoS or CIP to paste the prBodyLine.
4. End every commit with Agent, On-Behalf-Of and MC-Checkout as its last three lines. Name the TASK id in the PR title or branch.
5. Save evidence with mc_save_evidence before you open the PR, and put the MC-Checkout line in the PR body when you open it.
6. After you fix a PR body, re-run the failed compliance job. Never push an empty commit, and never close and reopen a PR, to restart a check.
7. After the last commit, call mc_complete_task with verificationCommands, rollback and the PR link. Then move the task's stage, or say why you cannot.
8. Mark a green PR ready and stop: only CIP's merge-queue session arms auto-merge or merges, with a merge commit into `main`. A PR that touches money, migrations or workflows, or reverts merged work, needs a signed MC merge approval of its exact head (mc_request_approval, merge scope).

- A plan that adds a project, or two or more buckets, goes through a draft: mc_save_draft, then mc_submit_draft. Its author accepts it in Mission Control.
<!-- plx-mc:handshake:end -->

## Workstream protocol

Sessions are disposable. What the next session or a teammate needs lives in
[`docs/workstreams/`](docs/workstreams/README.md) and travels with the code (Vince, 2026-10-26, TASK-3953).

- One workstream per branch and worktree. Flag any edit outside your workstream's scope before you make it. A one-PR change needs no workstream.
- At session start, read your workstream's note, `docs/workstreams/<slug>.md`. The index in `docs/workstreams/README.md` maps branches to slugs.
- Before you end a session, and after any significant decision, update that note in the same branch (`/handoff` in Claude Code and Cursor; `/resume-workstream` picks it up).
- Change the index row (owner, branch, status, dependencies) only when a workstream opens, closes or changes status.
- SharePoint `Agent Handoffs/` stays the system of record for durable handoffs. The note is the agents' working copy; when they disagree, SharePoint wins.

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
6. Import MC task discipline: every agent PR carries the Hub-minted `MC-Checkout: dsp_…` line (see Mission Control handshake above).

## Workflow discipline

- **Start of session:** `git checkout main && git pull`, then read the latest entries in `docs/sessions.md` (and `docs/learnings.md` before touching Amazon copy).
- **Workstreams:** work that spans more than one PR or session also keeps a note in `docs/workstreams/` (see Workstream protocol). `docs/sessions.md` stays the short per-session log.
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
