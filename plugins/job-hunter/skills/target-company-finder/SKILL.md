---
name: target-company-finder
description: Researches and maintains a ranked list of target companies in the user's Notion Target Companies database. Use when someone asks to build a target company list, find companies to apply to, or refresh their target list.
---

# Target Company Finder

Interview the user about what they're looking for, research a ranked company list, and write it into their Notion Target Companies database — safe to re-run to refresh the list without creating duplicates.

## Instructions

1. Search Notion for a database titled "Target Companies". If none exists, tell the user: "You don't have a Target Companies database yet — let me set one up first," then invoke the `notion-connector` skill to create the full tracker structure. Continue once it's created.

2. Ask the following, one at a time (wait for each response):
   1. "What area or domain are you targeting?" (industry or function, e.g. "fintech", "AI infrastructure", "gaming")
   2. "Any location preference — a specific city, hybrid, or fully remote?"
   3. "What stage of company: startup (seed/Series A), growth (Series B+), public/enterprise, or any?"
   4. "What role or level are you targeting?" (e.g. "Senior PM", "Staff Engineer")
   5. "How many companies should I find? (default 20 if no preference)"
   6. "Any companies I should exclude — already applied, not interested?" (optional, can skip)

3. Run web searches to build a candidate list matching the stated criteria. For each candidate company capture: name, category/domain, stage, location, careers page URL if found, and a one-line "why it fits" tied to the user's stated criteria. Exclude anything the user asked to exclude.

4. Group the candidates into three tiers based on fit against the stated criteria:
   - **Dream** — matches all criteria closely, most desirable
   - **Strong Fit** — matches most criteria well
   - **Backup** — plausible but a weaker match on one or more criteria

   Present the full list as a markdown table (Company | Tier | Category | Stage | Location | Why It Fits) to the user before writing anything. Ask: "Want me to write this to your Target Companies database, or should I adjust anything first?"

5. On approval, write to the Target Companies database:
   - For each candidate, query existing rows by company name (case-insensitive match).
   - **Match found:** update `Tier`, `Stage`, `Location`, `Why It Fits`. For `Notes`, append the new note rather than overwrite existing content. Never touch `Warm Contact` or `Status` — those are user-maintained.
   - **No match:** create a new row with `Status` = "Not Started" and all captured fields filled in.

6. Report a summary: "Added N, updated N." with the database link.

## Rules
- Never invent a company or fact about it — if web search can't confirm something (e.g. stage), leave that field blank rather than guessing.
- If fewer solid matches are found than requested, say so explicitly and return what was found — never pad the list with weak fits to hit the count.
- If a Notion write fails partway through the batch, report exactly which companies succeeded and which didn't. Do not retry silently.
- Never overwrite `Warm Contact` or `Status` on an existing row — those fields are user-maintained.
