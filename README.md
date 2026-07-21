# Mtn Brook Village Office — Project Finances

A single-page finance tracker for the Mtn Brook Village office build. No backend required — it's a static site meant to be hosted on GitHub Pages, matching the same pattern as `CMA_Office_Budget.html`.

## Files

- `index.html` — the app (open it directly, or host it)
- `ledger-data.json` — the committed backup of your ledger data. The app writes this file when you click **Save**; you then commit it to git so any other browser/device you open the app in can load the same starting data.

## How data storage works

- **Automatic save** — every edit is saved instantly to this browser's local database (IndexedDB). Close the tab, come back tomorrow, everything's still there — no action needed.
- **Save button** — exports the full ledger (including attached PDFs) as `ledger-data.json`. This is your durable, git-tracked backup and the way to sync data to a different browser or computer. Click it after a work session, then commit the updated file.
- **Load button** — manually load a `ledger-data.json` file, replacing what's in this browser.
- **Auto-load from repo** — when the app is hosted over `https://` (e.g. GitHub Pages) and a browser's local database is empty, it automatically fetches `ledger-data.json` from the same folder and loads it. If the browser already has entries and a newer repo file exists, you'll see a small "load it" link in the status strip instead of a silent overwrite.

Because PDFs are stored inline as base64 inside the entries, `ledger-data.json` can get large if you attach many big files — GitHub is fine with this up to typical repo sizes, but keep an eye on it if you're attaching dozens of large scans.

## Publishing on GitHub

1. Create (or reuse) a git repo in this folder and push it to GitHub.
2. In the repo's Settings → Pages, set the source to your default branch (root).
3. Visit the published URL — the app will auto-load `ledger-data.json` from the repo the first time you open it in a new browser.
4. After adding entries, click **Save**, move the downloaded `ledger-data.json` into this folder (replacing the old one), then `git add`, `git commit`, `git push`.

## Currency conversion

Any Estimated / Actual (Invoiced) / Paid field has a "convert currency" link underneath it. Pick a currency, enter the original amount, and click **Go** — it fetches the live rate from frankfurter.app and stores the USD-equivalent as the field's value, keeping the original amount and rate as a tooltip for reference.

## Dropbox completeness check

The app itself can't reach Dropbox (it's a static file with no server). Instead, click **Copy ledger manifest for the check** in the panel under the dashboard, then paste it to Claude and ask something like "check Dropbox against my ledger" — Claude will list your Cost Tracking folder and flag anything that isn't logged yet.
