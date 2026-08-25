---
name: notion-connector
description: Sets up or locates a job-search tracker structure in the user's Notion workspace — three databases for applications, target companies, and networking contacts. Use when someone asks to set up their job tracker in Notion, connect Notion, or when another skill needs a tracker database that doesn't exist yet.
---

# Notion Connector

Create (or reuse) a job-search tracker structure in the user's own Notion workspace. Never assume a specific existing page, database, or workspace — always search and confirm.

## Instructions

1. Confirm Notion access. Look for available tools whose names contain `notion-search`, `notion-create-database`, or `notion-fetch`. If none are available, tell the user: "I don't see a Notion connection available. Connect Notion first (via your connector settings, or `/mcp` in an interactive session), then ask me again." Stop.

2. Find or choose a parent page:
   - Search Notion for pages that look like an existing job-search hub (try queries like "job search", "next role", "career tracker").
   - If there's a clear single match, tell the user what you found and ask them to confirm using it, or provide a different page.
   - If there's no clear match, ask: "Should I create a new page called 'Job Search Tracker' to hold these databases, or do you want to point me at an existing page?" Wait for response.

3. For each of the three databases below, search under the chosen parent page for an existing database with a matching title (case-insensitive).
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

4. Report back to the user: for each of the 3 databases, whether it was created or reused, and its link.

## Rules
- Never hardcode a page ID, database ID, or URL — always search or ask.
- Never modify the schema of a database you reused.
- If creation fails partway (e.g. 2 of 3 databases created), report exactly which succeeded and which failed. Do not retry silently or guess at what went wrong.
- If asked to set this up again later, re-run the same search-then-create-if-missing logic — never create duplicates of a database that already exists under the chosen parent.
