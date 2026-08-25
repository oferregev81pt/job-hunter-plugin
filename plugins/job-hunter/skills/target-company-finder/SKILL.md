---
name: target-company-finder
description: Researches and maintains a ranked list of target companies in the user's job tracker (Notion or local files). Use when someone asks to build a target company list, find companies to apply to, or refresh their target list.
---

# Target Company Finder

Interview the user about what they're looking for, research a ranked company list, and write it into their Target Companies tracker — Notion or local files, whichever they use — safe to re-run to refresh the list without creating duplicates.

## Instructions

1. Determine where the user's tracker lives:
   - If a local `target-companies.csv` file already exists (check `./job-search-tracker/` and ask if unsure), use local files.
   - Else if a Notion database titled "Target Companies" exists, use Notion.
   - Else tell the user: "You don't have a target company tracker yet — let me set one up first," then invoke the `notion-connector` skill (which will itself ask whether to use Notion or local files). Continue once it's created, using whichever storage the user chose.

2. Ask the following, one at a time (wait for each response):
   1. "What area or domain are you targeting?" (industry or function, e.g. "fintech", "AI infrastructure", "gaming")
   2. "Any location preference — a specific city, hybrid, or fully remote?"
   3. "What stage of company: startup (seed/Series A), growth (Series B+), public/enterprise, or any?"
   4. "What role or level are you targeting?" (e.g. "Senior PM", "Staff Engineer")
   5. "How many companies should I find? (default 20 if no preference)"
   6. "Any companies I should exclude — already applied, not interested?" (optional, can skip)

3. Run web searches to build a candidate list matching the stated criteria. For each candidate company capture: name, category/domain, stage, location, careers page URL if found, target level (the user's stated answer, applied to all candidates since it's a search criterion, not something that varies per company), and a one-line "why it fits" tied to the user's stated criteria. Exclude anything the user asked to exclude.

4. Group the candidates into three tiers based on fit against the stated criteria:
   - **Dream** — matches all criteria closely, most desirable
   - **Strong Fit** — matches most criteria well
   - **Backup** — plausible but a weaker match on one or more criteria

   Present the full list as a markdown table (Company | Tier | Category | Stage | Location | Why It Fits) to the user before writing anything. Ask: "Want me to save this to your tracker, or should I adjust anything first?"

5. On approval, write the results — using whichever storage was determined in step 1:
   - For each candidate, look up an existing row by company name (case-insensitive match) — query Notion by property value, or read the local CSV and match on the `Company` column.
   - **Match found:** update `Tier`, `Category`, `Stage`, `Location`, `Careers URL`, `Target Level`, `Why It Fits`. For `Notes`, append the new note rather than overwrite existing content. Never touch `Warm Contact` or `Status` — those are user-maintained.
   - **No match:** add a new row (a new Notion page, or a new CSV line) with `Status` = "Not Started" and all captured fields filled in, including `Target Level`.
   - For local files, read the whole CSV, apply the match/update/append logic in memory, and write the full file back once — never append blindly without checking for an existing row first.

6. Report a summary: "Added N, updated N." with the database link, or the local file path.

## Rules
- Never invent a company or fact about it — if web search can't confirm something (e.g. stage), leave that field blank rather than guessing.
- If fewer solid matches are found than requested, say so explicitly and return what was found — never pad the list with weak fits to hit the count.
- If a write fails partway through the batch, report exactly which companies succeeded and which didn't. Do not retry silently.
- Never overwrite `Warm Contact` or `Status` on an existing row — those fields are user-maintained.
