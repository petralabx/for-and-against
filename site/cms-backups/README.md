# CMS backups

Weekly snapshots of Webflow collection content (Scents + Products), written by a scheduled GitHub Action hitting the Webflow API. One folder per run: `YYYY-MM-DD/scents.json`, `YYYY-MM-DD/products-collection.json`.

Purpose: (1) restore point — Webflow's export story is weak; (2) drift check against `data/products.json`.

**TODO:** build `.github/workflows/cms-backup.yml`. Webflow API token goes in Actions Secrets.
