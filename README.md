# Mountain Brook Village Office — Project Finances

A single-page finance report for the Mountain Brook Village office build. No backend required — it's a static site meant to be hosted on GitHub Pages. Built in the Carlisle Moore **Editorial** house style (rust/paper/forest palette, Columbia Titling / Aviano Sans / URW Antiqua / Franklin Gothic), laid out to match the Invoice 8348 cost-comparison sheet — a clean report, not a spreadsheet.

## Files

- `index.html` — the app (open it directly, or host it)
- `ledger-data.json` — the committed backup of your data. Once GitHub Sync is connected (see below), the app writes this file straight to your repo itself — you don't need to download or commit anything by hand.
- `assets/logo-wordmark.png`, `assets/house-mark.png` — brand assets used in the masthead. Keep them alongside `index.html`.

## How it's organized

The page is three sections, one per vendor/contract, plus a project-wide overview at the top:

**Overview** — a three-cell KPI band: Contract Value (with a TCC / GHT / Emory breakdown underneath), Paid to Date, and Remaining (contract value minus paid to date).

**01 · TCC General Contractors** — the cost-plus-15%, Net-15 contract. A KPI band (Contract Total, Invoiced to Date, Remaining) mirrors Invoice 8348's cost-comparison header. Below it, a **Line-Item Comparison** table transcribes the complete signed $580,302.64 contract (Exhibit B, "Carlisle Moore Schematic Estimate 3.7.26") line for line — every category, including allowance-only and excluded/by-owner items with no dollar figure — grouped into the same sections the contract itself uses (General Conditions, Sitework/Demo, Foundation, Masonry, Framing, Trim Carpentry, Exterior, Roofing/Flashing, Doors and Windows, Paint/Insulation/Sheetrock, Flooring and Tile, Cabinets/Tops, Appliances, Specialties, Electrical, Plumbing, Mechanical, and a Summary section for Overhead/Profit/Supervision and Contingency). Each row carries its Estimate, Billed to Date, a Variance that only shows a number when a category is actually over budget or out of contract scope, and a **Contract Notes** column with Exhibit B's own notes verbatim (e.g. "By Owner Per BM," "None Figured," "Solid Door Units"). Three contract-wide footnotes from the bottom of Exhibit B (excluded basement work, excluded electrical service rebuild, excluded fire alarm/sprinkler work) are called out beneath the table. A **Contingency Exposure** note auto-computes from the flagged variances. Below that, an **Invoices** table tracks TCC's individual draws — invoice number, amount, due date, status — since they're addressed to Bill Moore for the firm, separate from the line-item budget tracking. TCC line items intentionally don't carry a paid/unpaid status — that lives at the invoice level instead.

**02 · GHT Group** — Pre-Wire and Audio/Video, each with Estimate, Billed to Date, and Variance. The GHT Audio & Video line reflects the current $31,334.30 proposal (7.9.2026), not the earlier $30,055.01 figure.

**03 · Emory Ratliff Interiors** — the catch-all for everything outside TCC and GHT: her design fee/FF&E allowance plus every individual purchase logged from the Cost Tracking folder — Schumacher wallcovering, Visual Comfort sconces, Soho Electrical fixtures, Shiplights, Vitoch Interiors/Highland House furniture (paid in full, invoice IN-38088), AllSouth kitchen appliances (deposit paid, balance due before delivery), Brandino Brass hardware, and Lighting and Lamp Inc. fixtures. Each item has a one-click **Paid** checkbox — click it to mark an item paid in full (or click again to unmark); the box shows a green check when fully paid, a gold half-mark if only partially paid (set the exact partial amount via Edit). This is where "paid but no invoice yet" gets tracked — the **Awaiting Invoice** KPI tile totals anything checked as paid whose status is still "Paid — awaiting invoice," so it doesn't get lost before the invoice shows up.

## Editing data

Numbers in the tables are plain text, not input boxes — the report reads clean, the way the reference document does. To change something, click **edit** at the end of any row; a small form opens for just that item (Estimate, Billed to Date, Status, attachment, etc.) and **Save** updates the table. Use **+ Add Line Item** / **+ Add Invoice** / **+ Add Item** under each table to log something new. Attachments accept PDF, JPG, or PNG.

Dollar fields in the edit form show a thousands separator as you type (type `11610` and it becomes `11,610`) — cents are still accepted if you have them, they just don't show up in the report tables, which always round up to the nearest whole dollar for a cleaner read.

Any Emory item's Amount field has an inline currency converter (pick a currency, enter the original amount, **Convert → USD**) for foreign-currency purchases like the Soho Electrical Group order (GBP).

## How data storage works

- **Automatic save** — every change is saved instantly to this browser's local database (IndexedDB), and — once GitHub Sync is connected — pushed straight to your repo a few seconds later too. The header shows **Last saved &lt;date/time&gt;** so you can always see when something last actually landed somewhere durable.
- **Save button** — forces an immediate save: writes to this browser's local database right away (skipping the usual short debounce) and, if GitHub Sync is connected, pushes to GitHub immediately as well. Use it when you want to be sure something's landed before you close the laptop.
- **Auto-load from repo** — when the app is hosted over `https://` (e.g. GitHub Pages) and a browser's local database is empty, it automatically fetches `ledger-data.json` from the same folder and loads it. If the browser already has data and a newer repo file exists, you'll see a small "load it" link in the status strip instead of a silent overwrite.

Because attachments are stored inline as base64, `ledger-data.json` can get large if you attach many big files — GitHub is fine with this at typical sizes, but keep an eye on it if you're attaching dozens of large scans.

## Settings

Everything admin-ish — GitHub Sync and the Dropbox folder check — lives in a **Settings** section at the very bottom of the page, below Emory, so the main report stays focused on the numbers.

**GitHub Sync** pushes `ledger-data.json` directly to your GitHub repo — no downloading a file and committing it by hand. Fill in your repo owner, repo name, branch (usually `main`), the file path (`ledger-data.json` by default), and a **fine-grained personal access token** scoped to just that one repo with **Contents: Read and write** permission — the same kind of token you'd generate under GitHub → Settings → Developer settings → Fine-grained tokens — then click **Save GitHub Settings**. Click **Test Connection** first if you want to confirm the token and repo are right before relying on it. Once saved, **Push Now** and **Pull from GitHub** buttons appear.

After that first setup, every edit autosaves to this browser as always, and a few seconds later also pushes to GitHub automatically — that's the durable backup going forward. The **Save** button in the header does both immediately if you don't want to wait. **Pull from GitHub** fetches whatever is currently in the repo and loads it here (useful if you edited from another computer).

The token field shows the token as plain text (not dots) and is sized to fit a full token comfortably — it stays only in this browser's local database (IndexedDB), never written into `index.html`, never included in the exported `ledger-data.json`, and only ever sent over HTTPS to `api.github.com`. It won't end up committed to git history. Because a fine-grained token scoped to one repo has a small blast radius if it ever leaked, that's the right kind to use here — avoid a classic (account-wide) token. Click **disconnect** to remove it from this browser at any time. Anyone with access to this specific browser profile (dev tools, etc.) could read the stored token in plain text, so only set this up on a computer you trust.

The **Cost Tracking Folder** buttons here are the same folder-check and manifest-copy tools described below.

## A few things worth a second look

- **The full TCC Line-Item Comparison table was rebuilt from a fresh, careful re-read of Exhibit B** (all three pages, rotated and zoomed) and reconciles to the penny: every category's Total column across the whole contract sums to exactly the printed Subtotal ($462,752.20), and with Overhead/Profit/Supervision ($92,550.44) and Contingency ($25,000.00) added, to the printed Total ($580,302.64). Two earlier line items had been transcribed wrong in a prior pass — see the next bullet — and are now corrected.
- **"Electrical M/L" is $75,000, not excluded.** Exhibit B's Electrical section reads: Electrical M/L — 1 LS @ $75,000 = **$75,000**; Can Light Allowance — 50 EA @ $125 = **$6,250**; Decorative Fixture Allowance — excluded, By Owner Per BM. An earlier pass had these swapped (the $75,000 sitting on Can Light Allowance, with Electrical M/L marked excluded) — that was a misread on my part, and you were right to flag it. Both figures are now correct and billed-to-date ($2,025.24 against Electrical M/L) carried forward unchanged.
- **"Building permit / plan printing"** is now two separate lines, matching Exhibit B: **Building Permit** ($6,100 LS estimate) and **Plan Printing** (reimbursable, no fixed price). The combined actual cost to date ($6,234.89) is carried on the Building Permit line, with a note explaining it covers both.
- **Soho Electrical Group**, **Brandino Brass** (both the original hardware order and the two small follow-on invoices), and **Lighting and Lamp Inc.** are logged as "Ordered — confirm payment" — the saved documents show an order or invoice but no visible payment confirmation. Check with whoever placed each order and update via **edit** once you know.
- **AllSouth Appliance Group** shows the deposit ($9,437.40) as paid, with the $6,291.60 balance due before the 7/28/2026 delivery.
- **TCC Invoice 8348's date** (6/29/2026) is inferred from the 7/14 due date under Net-15 terms, since the source documents didn't show an explicit issue date — confirm against the paper invoice and correct it via **edit** if needed.
- **GHT Pre-Wire** now reflects its current Rev 0 proposal ($7,940.62, including tax) rather than the earlier $7,374.57 estimate — note in the app flags it as not yet signed.

## Checking the Cost Tracking folder for new files

The **Check Folder for New Files** button under Settings (at the bottom of the page) uses your browser's File System Access API to look directly at your local Cost Tracking folder — no server involved, nothing uploaded anywhere. The first click asks you to pick the folder once; after that it's a one-click permission re-confirmation. It compares filenames against everything already attached in the ledger and lists anything it doesn't recognize.

This only works in **Chrome or Edge** — Safari and Firefox don't support this API, and the button will tell you so if you click it there. It also only catches new *filenames* — it can't read dollar amounts out of a document. When it flags something new, copy the ledger manifest (button right next to it) and ask Claude to read the new file in with real numbers, the same way the current data was built.

## Publishing on GitHub

1. Create (or reuse) a git repo in this folder and push it to GitHub.
2. In the repo's Settings → Pages, set the source to your default branch (root).
3. Visit the published URL — the app will auto-load `ledger-data.json` from the repo the first time you open it in a new browser.
4. Set up **GitHub Sync** (above) with a fine-grained token for that repo, and every edit from then on pushes straight to `ledger-data.json` in the repo automatically — no manual commit needed.

## Dropbox completeness check

The app itself can't reach Dropbox (it's a static file with no server). Click **Copy Ledger Manifest** in the panel under the Overview KPIs, then paste it to Claude and ask something like "check Dropbox against my ledger" — Claude will list your Cost Tracking folder and flag anything that isn't logged yet.
