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

## 2026-08-05 — session log convention added

- **Who:** Stephen + Cursor
- **PR:** #10
- **Done:** Added this file; wired start/end-of-session discipline into `AGENTS.md`
- **Next:** Colleagues pull `main`, read latest entries before starting; append an entry with each meaningful PR
- **Watch:** Soft MC compliance — operator PRs do not need `MC-Checkout`
