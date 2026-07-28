# Learnings & issue log

Append-only. Two sections: experiments (what we tested, what happened) and known issues (defects found in live surfaces). Agents: check the issue log before reusing any live copy.

## Known issues — live Amazon listings (found 2026-07-14, from source spreadsheet audit)

| # | SKU(s) | Issue | Status |
|---|---|---|---|
| 1 | FNA750/755/756/757/758-75 | All five deodorant titles end "(Hinoki & Bergamot)" regardless of actual scent | OPEN |
| 2 | FNA750–758-75 | All deodorant descriptions describe Amber & Sandalwood scent | OPEN |
| 3 | FNA606-500 | Santal & Vetiver lotion bullet 4 describes Bergamot & Hinoki scent | OPEN |
| 4 | FNA605-2LP, FNA606-2LP | Titles say "(Santal & Vetiv)" (truncated); FNA605-2LP is actually Hinoki & Bergamot | OPEN |
| 5 | FNA300-2LP, FNA305-2LP | Body wash pouch descriptions describe "Santal & Vetiver" regardless of scent | OPEN |
| 6 | Multiple shampoo/pouch | Typo "STREGTHENS" in bullets | OPEN |
| 7 | FNA305-2LP, FNA306-2LP | Title typos: "Beramot", "Satal" | OPEN |
| 8 | FNA500/505/506-500, FNA605/606-2LP | "All Natural" claim in titles — compliance risk, see docs/compliance/claims.md | OPEN |
| 9 | FNA606-500, FNA600-2LP | Missing ASINs in source sheet — confirm whether listed | OPEN |
| 10 | Source xlsx | SKU List vs Financials disagree on gallon vs 4-gallon labeling for FNAx0x | OPEN |
| 11 | Brand-wide | "Hinoki & Bergamot" (internal) vs "Bergamot & Hinoki" (live titles) — pick one | OPEN |

## Experiments

_(none logged yet — every copy/pricing/ad test gets an entry: hypothesis, change, result, decision)_
