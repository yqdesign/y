# Cover Finder

Single-page app for deployers to find the best replacement when staff are absent.
Static site, no build step. Live data is stored in Supabase.

## Files

- `index.html` is the whole app (HTML, CSS and JS in one file).
- `vercel.json` has basic Vercel settings.
- `supabase/schema.sql` is the database schema (tables, open access policies, realtime). Already applied to the project.
- `data/` holds the November 2026 roster, scanner skills and leave records. These are already loaded into Supabase; the files are a backup.

## Deploy to Vercel

1. Push this folder to the GitHub repo.
2. In Vercel choose **Add New, then Project**, and import the repo.
3. Framework preset: **Other**. Leave build command and output directory empty.
4. Deploy. No environment variables are needed.

## Supabase

The app reads and writes four tables: `cf_roster`, `cf_skills`, `cf_leaves`, `cf_tasks`.
Everyone with the link sees changes live (Supabase Realtime, plus a quiet refresh every 20 seconds as a safety net).

- Project URL and the publishable key are set at the top of the Supabase block in `index.html` (`CF_SUPABASE`).
  The publishable key is meant to be public. Never put a secret or service_role key in this file.
- Access is **open**: the table policies let anyone with the link read and edit, including leave records.
  To restrict it later, add Supabase Auth and change the policies in `supabase/schema.sql` to require a signed-in user.
- If Supabase can't be reached, the app shows a notice and keeps the last data it loaded.
- Uploading a roster with **Replace roster** overwrites the roster row for everyone.

## Tabs

- **Staff finder**: roster grid, tap a cell when someone is out, ranked cover in a side drawer.
- **Roster**: read-only copy of the uploaded Excel sheet.
- **Leave analysis**: leave days, requests, frequency, gaps, trend and short-notice patterns per person, by leave type.
- **Leave request log**: add, edit and delete leave (deployers TYQ, DMC, NC, TKS only), plus trends for leave next to public holidays and weekends. Singapore public holidays 2025 to 2027 (MOM) are built in; add later years in the `PH` list near the top of the leave code.
- **Board** and **Approvals**: cover requests and team lead sign-off.

The "Working as" switch decides who can do what (deployers assign cover and edit leave, the team lead approves). It is not a login.
