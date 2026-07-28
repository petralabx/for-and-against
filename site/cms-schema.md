# Webflow CMS schema

**Last verified:** 2026-07-14 · Site: forandagainstbodycare.com

## Collections

Two nested collections:

1. **Scents** — one item per scent
2. **Products** — one item per product, referencing a Scent

## Critical field rules

- **Price Display is Plain Text**, not Number — this preserves the "$" symbol. Any generated CMS content must supply price as a string exactly matching `price_display` in `data/products.json` (e.g. `"$15.99"`). Never write a bare number.
- Content generated for the CMS must map to these fields — never freeform web copy.

## TODO — complete the field inventory

Pull the full field list per collection via the Webflow API (site is connected as an MCP tool) and record here: field name, slug, type, required, referenced-by. Until then, agents must fetch the live schema before writing CMS items rather than guessing field names.

## Sync rule

`data/products.json` is upstream of the CMS. Price or catalog changes: update products.json → then CMS. The scheduled CMS backup (below) doubles as the drift check.
