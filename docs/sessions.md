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
