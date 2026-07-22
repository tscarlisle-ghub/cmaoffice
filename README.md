# Mtn Brook Village Office — Project Finances

A single-page finance tracker for the Mtn Brook Village office build. No backend required — it's a static site meant to be hosted on GitHub Pages. Built in the Carlisle Moore **Editorial** house style (rust/paper/forest palette, Columbia Titling / Aviano Sans / URW Antiqua / Franklin Gothic).

## Files

- `index.html` — the app (open it directly, or host it)
- `ledger-data.json` — the committed backup of your ledger data. The app writes this file when you click **Save**; you then commit it to git so any other browser/device you open the app in can load the same starting data.
- `assets/logo-wordmark.png`, `assets/house-mark.png` — brand assets used in the masthead and empty state. Keep them alongside `index.html`.

## Fonts

The app loads Typekit at `https://use.typekit.net/wup0iix.css` (the same kit referenced in the Editorial System handoff). Adobe Fonts kits are domain-restricted — if the type doesn't render once this is live on GitHub Pages, add your `*.github.io` domain (and any custom domain) to that kit's allowed domains in the Adobe Fonts dashboard. Everything degrades gracefully to system serif/sans fallbacks in the meantime, so the app is fully usable either way.

## How data storage works

- **Automatic save** — every edit is saved instantly to this browser's local database (IndexedDB). Close the tab, come back tomorrow, everything's still there — no action needed.
- **Save button** — exports the full ledger (including attached PDFs) as `ledger-data.json`. This is your durable, git-tracked backup and the way to sync data to a different browser or computer. Click it after a work session, then commit the updated file.
- **Load button** — manually load a `ledger-data.json` file, replacing what's in this browser.
- **Auto-load from repo** — when the app is hosted over `https://` (e.g. GitHub Pages) and a browser's local database is empty, it automatically fetches `ledger-data.json` from the same folder and loads it. If the browser already has entries and a newer repo file exists, you'll see a small "load it" link in the status strip instead of a silent overwrite.

Attachments accept PDF, JPG, or PNG (handy for phone photos of receipts). Because they're stored inline as base64 inside the entries, `ledger-data.json` can get large if you attach many big files — GitHub is fine with this up to typical repo sizes, but keep an eye on it if you're attaching dozens of large scans.

## Adding files

Two ways to get files in:

- **Per-entry** — click **+ PDF / JPG** on any row's Attachment cell to attach one file to that entry.
- **+ Upload Files** — under the table, next to **+ Add Entry**. Pick one or several PDFs/JPGs/PNGs at once and the app creates one new entry per file, attached, with the vendor guessed from the filename (category defaults to "Other"). Review and fix up the vendor, category, and amounts afterward — the import doesn't try to read amounts out of the file itself.

## The ledger, as currently loaded

`ledger-data.json` ships pre-loaded with 37 entries, rebuilt directly from your actual source documents (TCC Invoice 8348 and its P&L-by-Job backup, the GHT Audio & Video Rev 0 proposal, and the signed office budget) rather than placeholders. It's organized to mirror the structure of the Invoice 8348 cost-comparison sheet: grouped by vendor/contract, each line carrying a real Estimate, a real Invoiced-to-date (Actual) figure, and a Variance.

**TCC (34 lines)** — every category from the signed $580,302.64 contract is broken out individually (Demolition, Framing Materials, Framing Labor, HVAC & Dehumidification, Dumpster/Portalet, Fireplace Surround, Interior Trim Material, Painting, Cabinets/Vanities, Equipment Rental/Fuel, Cleanup/Wages/Punch, Overhead/Profit/Supervision, Plans/Permits, Electrical M/L, plus the 20 not-yet-billed scope items — framing SF, structural engineering, allowances, doors/windows, insulation, sheetrock, flooring, signage, lighting, plumbing, contingency, etc.). Amounts came from Invoice 8348 and the P&L-by-Job backup attached to those rows; the 14 categories billed so far are marked Paid or Disputed depending on what TCC actually invoiced, the rest are Pending. These 34 lines sum to exactly the signed contract total ($580,302.64 estimated) and exactly Invoice 8348's billed total ($138,367.56 invoiced) — no double-counting between a summary row and its line items.

  One line, "Intumescent Paint Allowance (Floor)" ($5,818.54), is a reconciling entry: the original signed budget has an unexplained gap of that size between its own line items and its stated subtotal, and the budget sheet's own footnote flags an illegible allowance in that category. It's called out here so the number isn't silently absorbed elsewhere — worth a quick gut-check against your paper copy of the budget if you get a chance.

- **GHT Group — Pre-Wire**: $7,374.57 estimated, not yet invoiced.
- **GHT Group — Audio & Video**: $31,334.30 estimated (the current Rev 0 proposal from 7/9/2026 — this supersedes the earlier $30,055.01 figure from the original budget sheet), not yet invoiced. Proposal PDF attached to this row.
- **Emory Ratliff Interiors**: $89,633.17 estimated, not yet invoiced.

Grand total: **$708,644.68 estimated / $138,367.56 invoiced** across all 37 entries.

Two small line items — "Framed SF" and "Heated / Cooled SF" ($215.73 each) — look like odd leftovers from the original spreadsheet, but they're part of the signed budget's own reconciled subtotal, so they're kept in rather than dropped.

For future Dropbox syncs, ask Claude to check the folder against your ledger and either import new files the same way (reading real amounts out of them) or just flag what's missing — whichever you'd rather do at the time.

## Publishing on GitHub

1. Create (or reuse) a git repo in this folder and push it to GitHub.
2. In the repo's Settings → Pages, set the source to your default branch (root).
3. Visit the published URL — the app will auto-load `ledger-data.json` from the repo the first time you open it in a new browser.
4. After adding entries, click **Save**, move the downloaded `ledger-data.json` into this folder (replacing the old one), then `git add`, `git commit`, `git push`.

## Currency conversion

Any Estimated / Actual (Invoiced) / Paid field has a "convert currency" link underneath it. Pick a currency, enter the original amount, and click **Go** — it fetches the live rate from frankfurter.app and stores the USD-equivalent as the field's value, keeping the original amount and rate as a tooltip for reference.

## Dropbox completeness check

The app itself can't reach Dropbox (it's a static file with no server). Instead, click **Copy ledger manifest for the check** in the panel under the dashboard, then paste it to Claude and ask something like "check Dropbox against my ledger" — Claude will list your Cost Tracking folder and flag anything that isn't logged yet.

## Dashboard notes

- **Due / Overdue** tile totals anything due within the next 30 days *plus* anything already past its due date (so it won't silently hide overdue balances just because the due date has passed) — hover the ⓘ for the exact definition. Overdue rows are highlighted in the table with a rust-tinted background.
- The table sorts by **vendor** by default, so all of a contract's line items group together (matching the layout of the Invoice 8348 comparison sheet). Click any column header to re-sort.
