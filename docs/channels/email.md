# Channel: Email

**Last verified:** 2026-07-14 — playbook stub, expand as the program launches

- Voice per [brand/voice.md](../brand/voice.md); subject lines short + declarative, no clickbait, no emoji walls.
- Product links point to Amazon listings (off-site traffic strategy) unless the campaign is explicitly a website play.
- Prices pulled from `data/products.json` at send time — never hardcode from memory.
- Approved sends get archived in `copy/email/`; results (open/CTR/revenue) logged in `docs/learnings.md`.
- **TODO:** ESP choice, list segments, welcome flow, refill-reminder flow (2L pouch buyers are the natural repeat cohort).
