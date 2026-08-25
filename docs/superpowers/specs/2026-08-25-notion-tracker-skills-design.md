# Design: Notion Connector + Target Company Finder skills

## Context

The `job-hunter` plugin (plugins/job-hunter/) currently ships 7 skills covering
research, fit analysis, CV/cover-letter generation, and interview prep — all
conversational, no persistence. The user wants to add persistence to Notion:
a generic way to stand up a job-search tracker structure in Notion, and a
skill that researches and maintains a "top N target companies" list inside it.

This is genericized for any plugin user's own Notion workspace — it must not
reference or link to the plugin author's personal boards/IDs. Every skill
creates or locates structure fresh, in whatever workspace the user has
connected.

A third skill (LinkedIn connection search per target company) was considered
and explicitly dropped: there's no LinkedIn API/MCP tool available, and
scraping LinkedIn violates its ToS, so it's out of scope for this change.

## Goals

1. A `notion-connector` skill that creates (or reuses) a 3-database job-search
   tracker structure in the user's Notion workspace: Applications Tracker,
   Target Companies, Networking Contacts.
2. A `target-company-finder` skill that interviews the user about target
   criteria, researches a ranked company list, and writes/updates it in the
   Target Companies database — safe to re-run to refresh the list.

## Non-goals

- No LinkedIn connection search (dropped per user decision).
- No automated syncing of Applications Tracker from job boards — that DB is
  created for future use (e.g. by application-packager) but this change does
  not wire any skill to write to it automatically.
- No hardcoded Notion page/database IDs, URLs, or workspace references
  belonging to any specific user.

## Skill 1: `notion-connector`

**Location:** `plugins/job-hunter/skills/notion-connector/SKILL.md`

**Trigger description:** Sets up or locates a job-search tracker structure in
the user's Notion workspace — three databases (applications, target
companies, networking contacts). Use when someone asks to set up their job
tracker in Notion, connect Notion, or when another skill needs a tracker
database that doesn't exist yet.

**Flow:**

1. Confirm a Notion MCP connector is available (look for tools whose names
   contain `notion-search` / `notion-create-database` / `notion-fetch`). If
   none are available, tell the user to connect Notion (via their connector
   settings or `/mcp`) and stop — do not proceed.
2. Determine the parent page:
   - Search the workspace (`notion-search`) for a page that looks like an
     existing job-search hub (e.g. matching "job search", "next role",
     "career").
   - If a clear candidate exists, confirm with the user before using it.
   - Otherwise ask the user: use an existing page (they provide a link/title)
     or create a new top-level page (default title: "Job Search Tracker").
3. For each of the three databases below: search under the chosen parent for
   an existing database with a matching title. If found, reuse it as-is (do
   not modify its schema) and report it. If not found, create it with the
   schema specified below.
4. Report all three database links/titles back to the user, noting which
   were created vs. reused.

**Schemas to create:**

*Applications Tracker* — title: `Role / Company` (title type); `Company`
(text); `Status` (status: Not Started, Applied, In Progress, Interviewing,
Offer, Rejected, Withdrawn); `Date Applied` (date); `Next Step` (text);
`Next Step Date` (date); `Source` (select: Referral, LinkedIn, Direct Apply,
Recruiter, Other); `Salary Range` (text); `Notes` (text).

*Target Companies* — title: `Company` (title type); `Category` (text,
free-form — populated per user's domain, not a fixed option set);
`Stage` (select: Seed, Series A, Series B, Growth, Public, Other);
`Location` (text); `Target Level` (multi-select, seeded empty — populated
as the finder skill runs); `Tier` (select: Dream, Strong Fit, Backup);
`Status` (status: Not Started, In Progress, Done); `Careers URL` (url);
`Why It Fits` (text); `Warm Contact` (text); `Notes` (text).

*Networking Contacts* — title: `Name` (title type); `Company` (text);
`Role` (text); `Relationship` (select: Former Colleague, Friend, Community
Member, Recruiter, Cold); `Status` (select: To Reach Out, Reached Out, Call
Scheduled, Call Done, Ongoing); `Last Contact` (date); `Notes` (text).

**Error handling:** if database creation fails partway (e.g. 2 of 3
succeed), report exactly which ones succeeded/failed and do not retry
silently. Never guess at a parent page — if search is ambiguous, ask.

## Skill 2: `target-company-finder`

**Location:** `plugins/job-hunter/skills/target-company-finder/SKILL.md`

**Trigger description:** Researches and maintains a ranked list of target
companies in the user's Notion Target Companies database. Use when someone
asks to build a target company list, find companies to apply to, or refresh
their target list.

**Flow:**

1. Search Notion for a database titled "Target Companies". If none exists,
   tell the user and invoke the `notion-connector` skill to create the full
   structure, then continue.
2. Ask the following, one at a time (multiple choice where sensible):
   - Target area/domain (industry or functional focus, e.g. "fintech", "AI
     infra", "gaming") — open text.
   - Preferred location(s) — city, hybrid, or fully remote.
   - Company stage preference — Startup (seed/Series A), Growth (Series B+),
     Public/Enterprise, or Any.
   - Target role/level (e.g. "Senior PM", "Staff Engineer").
   - How many companies to find — default 20 if the user has no preference.
   - Optional: any companies to exclude (already applied, not interested).
3. Research via web search to build the candidate list. For each company
   capture: name, category/domain, stage, location, careers URL if found,
   and a one-line "why it fits" tied to the user's stated criteria.
4. Group results into three tiers — Dream, Strong Fit, Backup — based on how
   closely each matches the stated criteria, and present the full list to
   the user as a table before writing anything.
5. On user approval (allow edits/removals first): write to the Target
   Companies database.
   - Query existing rows by company name (case-insensitive).
   - For a match: update Tier, Stage, Location, Why It Fits, Notes (append
     rather than overwrite Notes if it already has content) — never touch
     Warm Contact or Status, since those are user-maintained fields.
   - For no match: create a new row with Status = "Not Started".
6. Report a summary: N added, N updated, and the database link.

**Error handling:** if web search returns fewer solid matches than
requested, say so explicitly and return what was found rather than padding
the list with weak fits. If a Notion write fails partway through the batch,
report exactly which companies succeeded and which didn't — don't silently
skip or retry.

## Data flow summary

```
User → target-company-finder → (checks) Notion "Target Companies" exists?
                                    │ no
                                    ▼
                              notion-connector → creates 3 DBs, returns links
                                    │
                                    ▼
                    target-company-finder resumes, asks criteria questions
                                    │
                                    ▼
                         web research → tiered list → user approval
                                    │
                                    ▼
                    write/update rows in Target Companies DB → summary
```

## Testing / validation approach

Since these are prompt-based skills (no code to unit test), validation is
manual: run `notion-connector` against a scratch Notion workspace/page and
confirm all three databases are created with the correct schema and
re-running it reuses rather than duplicates them; run `target-company-finder`
end-to-end and confirm dedup-by-name works on a second run (updates existing
rows instead of creating duplicates).

## Open questions / follow-ups (not blocking)

- Whether `application-packager` should eventually offer to log its output
  into the Applications Tracker DB — deferred, out of scope here.
