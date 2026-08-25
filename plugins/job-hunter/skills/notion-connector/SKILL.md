---
name: notion-connector
description: Sets up or locates a job-search tracker structure — three trackers for applications, target companies, and networking contacts — either in the user's Notion workspace or as local files on their machine. Use when someone asks to set up their job tracker, connect Notion, or when another skill needs a tracker that doesn't exist yet.
---

# Notion Connector

Create (or reuse) a job-search tracker structure, in whichever storage the user prefers. Notion is optional — always offer a local-files alternative, and never assume a specific existing page, database, folder, or workspace.

## Instructions

1. Ask the user: "Where should I set up your job-search tracker — in Notion, or as local files on this machine (no external account or connection needed)?" Wait for response. If the user has already stated a preference earlier in the conversation, use it without asking again.

2. **If the user chose local files:**
   1. Search broadly for an existing tracker first — e.g. `find . -maxdepth 4 -iname "target-companies.csv" -o -maxdepth 4 -iname "applications-tracker.csv" -o -maxdepth 4 -iname "networking-contacts.csv"` — so a previously chosen custom folder is found instead of assuming none exists. If found, use that folder and skip to step 2.3.
   2. Otherwise ask: "Where should I put these files? (default: `./job-search-tracker/` in the current directory)" Wait for response, or use the default if they have no preference.
   3. For each of the three trackers below, check whether its matching CSV file already exists in that folder — filenames map to trackers as: `applications-tracker.csv` ↔ Applications Tracker, `target-companies.csv` ↔ Target Companies, `networking-contacts.csv` ↔ Networking Contacts.
      - If found: reuse it as-is. Do not rewrite or reorder its header row or existing rows. Note it as "reused."
      - If not found: create it with a header row containing exactly the backtick-quoted field name from each bullet below (strip the type/options annotation after the `—`), in the same order as listed, no data rows. Write it as proper CSV: fields containing a comma, quote, or newline must be wrapped in double quotes with embedded double quotes doubled (RFC 4180) — use a real CSV writer (e.g. Python's `csv` module via the Bash tool) rather than joining strings with commas, since free-text fields like "Why It Fits" will routinely contain commas. Note it as "created."
   4. Report back the folder path and which files were created vs. reused. Skip straight to the end — do not run the Notion steps below.

3. **If the user chose Notion:** confirm access first. Look for available tools whose names contain `notion-search`, `notion-create-database`, or `notion-fetch`. If none are available, tell the user: "I don't see a Notion connection available. Connect Notion first (via your connector settings, or `/mcp` in an interactive session), or I can set this up as local files instead — want me to do that?" If they switch to local, go to step 2. Otherwise stop.

4. Find or choose a parent page:
   - Search Notion for pages that look like an existing job-search hub (try queries like "job search", "next role", "career tracker").
   - If there's a clear single match, tell the user what you found and ask them to confirm using it, or provide a different page.
   - If there's no clear match, ask: "Should I create a new page called 'Job Search Tracker' to hold these databases, or do you want to point me at an existing page?" Wait for response.

5. If the user chose to create a new page, use an available `notion-create-pages`-style tool to create a page titled "Job Search Tracker" at the workspace top level (or under a location the user specifies). Use the resulting page as the parent for step 6. If the user instead pointed you at an existing page, use that page as the parent.

6. For each of the three databases below, search under the chosen parent page for an existing database with a matching title (case-insensitive).
   - If found: reuse it. Do not modify its schema. Note it as "reused."
   - If not found: create it with the exact schema given. Note it as "created."

   **Applications Tracker**
   - `Role / Company` — title
   - `Company` — text
   - `Status` — status, options: Not Started, Applied, In Progress, Interviewing, Offer, Rejected, Withdrawn
   - `Date Applied` — date
   - `Next Step` — text
   - `Next Step Date` — date
   - `Source` — select, options: Referral, LinkedIn, Direct Apply, Recruiter, Other
   - `Salary Range` — text
   - `Notes` — text

   **Target Companies**
   - `Company` — title
   - `Category` — text
   - `Stage` — select, options: Seed, Series A, Series B, Growth, Public, Other
   - `Location` — text
   - `Target Level` — multi-select, no preset options (populated as rows are added)
   - `Tier` — select, options: Dream, Strong Fit, Backup
   - `Status` — status, options: Not Started, In Progress, Done
   - `Careers URL` — url
   - `Why It Fits` — text
   - `Warm Contact` — text
   - `Notes` — text

   **Networking Contacts**
   - `Name` — title
   - `Company` — text
   - `Role` — text
   - `Relationship` — select, options: Former Colleague, Friend, Community Member, Recruiter, Cold
   - `Status` — select, options: To Reach Out, Reached Out, Call Scheduled, Call Done, Ongoing
   - `Last Contact` — date
   - `Notes` — text

7. Report back to the user: for each of the 3 databases, whether it was created or reused, and its link.

## Rules
- Notion is always optional. Never set up Notion without asking first, and never treat local files as a fallback of last resort — it's an equally valid first choice.
- Never hardcode a page ID, database ID, folder path, or URL — always search, ask, or use the stated default.
- Never modify the schema of a database (or the header row of a local CSV) you reused.
- Always write local CSVs with proper quoting/escaping (RFC 4180) via a real CSV library, never by joining fields with commas manually — a "Why It Fits" sentence containing a comma will silently corrupt a naively-written row.
- If creation fails partway (e.g. 2 of 3 databases or files created), report exactly which succeeded and which failed. Do not retry silently or guess at what went wrong.
- If asked to set this up again later, re-run the same search-then-create-if-missing logic for whichever storage was chosen — never create duplicates of a database or file that already exists.
