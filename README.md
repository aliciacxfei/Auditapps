# Audit Tracker — SFRS(I) & SSA

A single-page audit testing app for financial data. Upload Excel ledgers or trial balances, run automated audit tests, and track the exceptions as issues. Issues are ranked by risk and priority and linked to the relevant Singapore Standards on Auditing (SSA) and SFRS(I) paragraphs.

## Features
- **Issue tracker**: amount, risk category, assertion, risk level, priority (P1–P3) and status. Issues are ranked by risk → priority → amount. Risk levels are suggested against overall materiality, performance materiality and the clearly trivial threshold (SSA 320 / SSA 450).
- **Where to find it**: each issue records the file, sheet and cells. *Locate in file* opens the spreadsheet with that row and cell highlighted.
- **Standards links**: each issue links to SSA / SFRS(I) paragraphs. Clicking a reference opens the paragraph highlighted, with its key phrase marked.
- **Excel uploader** (.xlsx, .xls, .csv) with automatic column mapping and 10 tests:
  - debits = credits
  - duplicates
  - round amounts
  - weekend postings
  - cut-off around year end
  - missing descriptions or references
  - items ≥ performance materiality
  - Benford's law
  - unusual trial balance balances
  - text in amount columns

## Data storage
Everything is stored in the browser's **localStorage**. No data is sent to a server. Data stays in the one browser where it was entered, so use **Export backup / Import backup** on the Overview page to move it or keep a copy.

## Deploy to Vercel
1. In Vercel, choose **Add New → Project** and import `aliciacxfei/Auditapps`.
2. Framework preset: **Other**. Leave the build command and output directory empty (this is a static site).
3. Click **Deploy**. Each push to `main` redeploys automatically.

## Run locally
Open `index.html` in a browser, or serve the folder with `npx serve .`

## Try it
Click **Load sample engagement** in the app. You can also upload `samples/Sample_Audit_Workbook.xlsx`; first set the financial year end to 31 Dec 2026 on the Overview page.

## External dependencies (CDN)
- [SheetJS](https://sheetjs.com/) `xlsx` 0.18.5 from cdnjs, for reading Excel files
- IBM Plex Sans / Mono from Google Fonts

## Disclaimer
The standards paragraph summaries are paraphrased for working reference. They are not the authoritative text. Check the wording and paragraph numbers against the current SSAs (ACRA) and SFRS(I) (Accounting Standards Committee) before relying on them in working papers.
