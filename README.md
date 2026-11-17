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

## Sign-in & user management (Supabase)
- Users sign in with a **username + password**. Passwords are stored as bcrypt hashes in Supabase Auth (`auth.users`). Internally, each username maps to `<username>@auditapps.example.com`.
- New users must choose their own password the first time they sign in.
- **Admins** have a **Users** page where they can add users with an initial password, edit names and roles, reset passwords, and deactivate or reactivate users. **Members** can run tests and manage issues.
- Seeded accounts (initial password `password`, changed at first sign-in):

  | Username | Role |
  |---|---|
  | priya | admin |
  | dan | member |
  | marcus | member |

## Data storage
All data is stored in Supabase and shared by every signed-in user. Changes sync in real time.

| Table | Purpose |
|---|---|
| `profiles` | one row per user (username, full name, role, active, must_change_password), linked 1:1 to `auth.users` |
| `engagements` | shared engagement settings: entity, year end, materiality (single row) |
| `issues` | issue tracker rows, with `created_by` / `updated_by` audit stamps |
| `audit_files` | uploaded workbooks, with parsed sheets stored as JSONB |

Security:
- **Row Level Security** is on for every table. Anonymous visitors can't read anything.
- User management runs through `SECURITY DEFINER` functions (`admin_create_user`, `admin_set_password`, `admin_update_user`, `admin_set_active`), and each one checks that the caller is an admin.
- The browser only holds the **publishable** key, which is safe to expose.
- If someone used the app before sign-in was added, their old browser data is offered for import into the workspace the first time they sign in.

## Deploy to Vercel
1. In Vercel, choose **Add New → Project** and import `aliciacxfei/Auditapps`.
2. Framework preset: **Other**. Leave the build command and output directory empty (this is a static site).
3. Click **Deploy**. Each push to `main` redeploys automatically.

## Run locally
Open `index.html` in a browser, or serve the folder with `npx serve .`

## Try it
Sign in, then click **Load sample engagement**. You can also upload `samples/Sample_Audit_Workbook.xlsx`; first set the financial year end to 31 Dec 2026 on the Overview page.

## External dependencies (CDN)
- [supabase-js](https://github.com/supabase/supabase-js) 2.45.4 from jsDelivr, for auth and data
- [SheetJS](https://sheetjs.com/) `xlsx` 0.18.5 from cdnjs, for reading Excel files
- IBM Plex Sans / Mono from Google Fonts

## Disclaimer
The standards paragraph summaries are paraphrased for working reference. They are not the authoritative text. Check the wording and paragraph numbers against the current SSAs (ACRA) and SFRS(I) (Accounting Standards Committee) before relying on them in working papers.
