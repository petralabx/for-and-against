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

## 2026-08-11 — Google Ads Phase 1/2 strategy written into paid-ads.md

- **Who:** Greg + Claude (claude.ai, GitHub MCP connector)
- **PR:** n/a (see `docs/google-ads-phase1-strategy` branch)
- **Done:** Filled in `docs/channels/paid-ads.md`'s open TODO — Phase 1 campaign structure (Competitor Conquesting: Salt & Stone/Ouai/Necessaire; Scent & Category: H&B/S&V/Category-Body Wash), budget tiers ($50/day launch, $100/day scale), Amazon Attribution tracking architecture for Google-driven traffic (separate `For & Against — Google Ads` campaign, per-theme ad groups for keyword-level routing), and the weekly reconciliation workflow.
- **Next:** Category — Shampoo and Category — Lotion ad groups are documented but intentionally held for Phase 2 until Campaign 1–2 prove payback. No deodorant keyword data exists yet in `data/keywords/` — needs a direct Google Keyword Planner pull since H&B/S&V deodorant are best sellers but have no seed file. `docs/channels/amazon-attribution.md` doesn't exist yet — this doc references it as a TODO link.
- **Watch:** All keyword volumes in this doc are Amazon-side (Jungle Scout), explicitly flagged as prioritization signals only — re-run every shortlist through Google Keyword Planner before actually launching, since Google and Amazon search behavior diverge (e.g. scent-plus-product terms are much thinner on Google). Amber & Sandalwood is excluded from all campaign planning per the discontinuation decision.

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
