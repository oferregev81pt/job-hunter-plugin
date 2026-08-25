# Notion Tracker Skills Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add two skills to the `job-hunter` plugin — `notion-connector` (creates/reuses a 3-database job-search tracker in the user's own Notion workspace) and `target-company-finder` (interviews the user, researches target companies, writes them into the Target Companies database) — and wire them into the plugin's README and manifest.

**Architecture:** These are prompt-based Claude Code skills (a `SKILL.md` file per skill directory, no application code). "Implementation" means authoring the SKILL.md instruction files precisely per the approved design spec, then validating them against a real Notion workspace via manual smoke test (there is no unit-test framework for skill prompts — the validation step below is the equivalent of the test suite for this kind of change).

**Tech Stack:** Markdown SKILL.md files (YAML frontmatter + instructions), consumed by Claude Code's `Skill` tool; Notion MCP connector tools (`notion-search`, `notion-create-database`, `notion-fetch`, `notion-create-pages`, `notion-update-data-source`, `notion-query-data-sources`) invoked at runtime by whoever uses the skill.

**Reference:** Design spec at `docs/superpowers/specs/2026-08-25-notion-tracker-skills-design.md`.

---

### Task 1: Create the `notion-connector` skill

**Files:**
- Create: `plugins/job-hunter/skills/notion-connector/SKILL.md`

- [ ] **Step 1: Write the skill file**

Create `plugins/job-hunter/skills/notion-connector/SKILL.md` with exactly this content:

```markdown
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
```

- [ ] **Step 2: Verify frontmatter is valid YAML and file structure matches sibling skills**

Run:
```bash
head -5 plugins/job-hunter/skills/notion-connector/SKILL.md
```
Expected: Shows `---`, `name: notion-connector`, `description: ...`, `---` — matching the frontmatter shape used by `plugins/job-hunter/skills/company-researcher/SKILL.md`.

- [ ] **Step 3: Commit**

```bash
git add plugins/job-hunter/skills/notion-connector/SKILL.md
git commit -m "feat(job-hunter): add notion-connector skill"
```

---

### Task 2: Create the `target-company-finder` skill

**Files:**
- Create: `plugins/job-hunter/skills/target-company-finder/SKILL.md`

- [ ] **Step 1: Write the skill file**

Create `plugins/job-hunter/skills/target-company-finder/SKILL.md` with exactly this content:

```markdown
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
```

- [ ] **Step 2: Verify frontmatter is valid YAML and file structure matches sibling skills**

Run:
```bash
head -5 plugins/job-hunter/skills/target-company-finder/SKILL.md
```
Expected: Shows `---`, `name: target-company-finder`, `description: ...`, `---`.

- [ ] **Step 3: Commit**

```bash
git add plugins/job-hunter/skills/target-company-finder/SKILL.md
git commit -m "feat(job-hunter): add target-company-finder skill"
```

---

### Task 3: Update the plugin README and manifest

**Files:**
- Modify: `plugins/job-hunter/README.md`
- Modify: `plugins/job-hunter/.claude-plugin/plugin.json`

- [ ] **Step 1: Add the two new skills to the README table**

In `plugins/job-hunter/README.md`, the skills table currently ends with the `application-packager` row followed by a blank line and then `## How to use`. Insert two new rows immediately after the `application-packager` row (before the blank line), so the table reads:

```markdown
| **application-packager** | Everything above in one conversation — full application package | "Build my full application package" |
| **notion-connector** | Sets up a job-search tracker (applications, target companies, networking contacts) in your own Notion workspace | "Set up my job tracker in Notion" |
| **target-company-finder** | Researches and maintains a ranked target-company list in your Notion tracker | "Find me 20 target companies" |
```

- [ ] **Step 2: Add a short note about the Notion requirement**

At the end of `plugins/job-hunter/README.md`, after the existing "How to use" paragraph, add:

```markdown

## Notion integration

`notion-connector` and `target-company-finder` need a Notion MCP connector available in your Claude Code session. If you haven't connected Notion yet, connect it first (via your connector settings, or `/mcp` in an interactive session) — everything else in the plugin works without it.
```

- [ ] **Step 3: Bump the plugin version and add a keyword**

In `plugins/job-hunter/.claude-plugin/plugin.json`, change:
```json
  "version": "0.1.0",
```
to:
```json
  "version": "0.2.0",
```
and change:
```json
  "keywords": ["job-search", "cv", "cover-letter", "interview-prep", "career"]
```
to:
```json
  "keywords": ["job-search", "cv", "cover-letter", "interview-prep", "career", "notion"]
```

- [ ] **Step 4: Verify plugin.json is valid JSON**

Run:
```bash
python3 -m json.tool plugins/job-hunter/.claude-plugin/plugin.json > /dev/null && echo OK
```
Expected: `OK`

- [ ] **Step 5: Commit**

```bash
git add plugins/job-hunter/README.md plugins/job-hunter/.claude-plugin/plugin.json
git commit -m "docs(job-hunter): document notion-connector and target-company-finder in README, bump version"
```

---

### Task 4: Manual validation against a real Notion workspace

This project has no automated test framework — skills are prompt files, not code. Validation is a manual smoke test run against a **scratch/test Notion page**, not any existing personal board.

**Files:** none (validation only)

- [ ] **Step 1: Validate `notion-connector` creates fresh structure**

In a Claude Code session with the Notion connector active and this plugin loaded, say: "Set up my job tracker in Notion" and, when asked for a parent page, direct it to create a new page (accept the "Job Search Tracker" default, or point it at a disposable scratch page you created for this test — do not point it at a real personal board).

Expected: three databases are created — Applications Tracker, Target Companies, Networking Contacts — each with the exact fields listed in Task 1 Step 1. Confirm field names, types, and select/status options match by opening each database in Notion.

- [ ] **Step 2: Validate reuse-not-duplicate on re-run**

Run the same request again against the same parent page.

Expected: the skill reports all three databases as "reused," and no duplicate databases are created under the parent page.

- [ ] **Step 3: Validate `target-company-finder` end-to-end**

Say: "Find me target companies" (or similar). Answer the interview questions with test criteria (e.g. domain "fintech", location "Tel Aviv", stage "growth", role "Senior PM", count "5" to keep the test small).

Expected: the skill presents a markdown table of 5 candidates grouped by tier before writing anything, asks for approval, and — once approved — creates 5 new rows in the Target Companies database with `Status` = "Not Started" and the other captured fields filled in.

- [ ] **Step 4: Validate dedup-on-rerun updates instead of duplicating**

Manually edit one of the 5 rows in Notion: set its `Warm Contact` to a test value (e.g. "Test Contact") and its `Status` to "In Progress". Run the `target-company-finder` skill again with the same domain/criteria so at least one of the same companies is proposed again, and approve the write.

Expected: the existing row's `Tier`/`Stage`/`Location`/`Why It Fits` are updated and `Notes` is appended to (not overwritten), while `Warm Contact` ("Test Contact") and `Status` ("In Progress") are left untouched. No duplicate row is created for that company.

- [ ] **Step 5: Clean up**

Archive or delete the scratch page and its databases created during validation, since they were for testing only.

- [ ] **Step 6: Record validation result**

If all four expectations above held, note in the PR/commit description (or tell the user directly) that manual validation passed. If anything diverged, fix the relevant SKILL.md in Task 1 or Task 2 and re-run the affected validation steps before considering the plan complete.

---

## Self-review notes

- **Spec coverage:** Task 1 covers the `notion-connector` schema/flow section of the spec verbatim; Task 2 covers `target-company-finder`'s question flow, tiering, and dedup-on-write logic verbatim; Task 3 covers documentation/discoverability (README, plugin.json) which the spec implies but doesn't spell out as a task — added here since the plugin's existing skills are all documented in the README and a new skill that isn't listed there is effectively undiscoverable. Task 4 covers the spec's "Testing / validation approach" section.
- **Non-goals respected:** no task wires `application-packager` or any other existing skill to write to the Applications Tracker DB; no task hardcodes any page/database ID.
- **Type/naming consistency:** field names (`Role / Company`, `Tier`, `Warm Contact`, etc.) are identical across Task 1's schema definition and Task 2's write logic, and match the design spec.
