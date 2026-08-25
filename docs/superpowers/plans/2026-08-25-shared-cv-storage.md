# Shared CV Storage Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add local CV storage/reuse (`./job-search-tracker/cv.md`) to the job-hunter plugin so users don't have to re-paste their CV every time they run a skill.

**Architecture:** Prompt-only change across 6 existing `SKILL.md` files — no application code. Five skills (`application-packager`, `cover-letter-writer`, `fit-analyzer`, `cv-tailor`, `interview-prep`) get their "paste your CV" step replaced with a check-saved-file-first-then-paste-and-save flow. `cv-builder` gets a new final step that saves its generated output to the same file.

**Tech Stack:** Markdown SKILL.md files consumed by Claude Code's `Skill` tool. No new tools/dependencies — reuses the local-file convention `notion-connector` already established.

**Reference:** Design spec at `docs/superpowers/specs/2026-08-25-shared-cv-storage-design.md`.

---

### Task 1: Add CV load/save flow to `application-packager`

**Files:**
- Modify: `plugins/job-hunter/skills/application-packager/SKILL.md`

- [ ] **Step 1: Replace the CV input step**

Find this exact text:
```markdown
1. "Please paste your CV below." (Wait.)
```

Replace it with:
```markdown
1. Check whether `./job-search-tracker/cv.md` exists and is non-empty.
   - If found: say "Found your saved CV — using it." and use its contents. Do not ask the user to paste anything — skip to the next input.
   - If not found: ask "Please paste your CV below." (Wait.) Once given, save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist).
   - If the user pastes a new CV unprompted later even though a saved one was used, overwrite `cv.md` with the new content immediately — no confirmation needed.
```

- [ ] **Step 2: Add a rule against overwriting cv.md with generated output**

Find this exact text (in the `## Rules` section):
```markdown
- Keep the full output as one document so the user can copy it all at once
```

Replace it with:
```markdown
- Keep the full output as one document so the user can copy it all at once
- Never overwrite `./job-search-tracker/cv.md` with the tailored CV generated in Step 4 — only the original source CV loaded or pasted in Step 1 belongs there
```

- [ ] **Step 3: Verify the file reads cleanly**

Run:
```bash
grep -n "cv.md" plugins/job-hunter/skills/application-packager/SKILL.md
```
Expected: 3 matching lines (the load/save step and the two rule mentions).

- [ ] **Step 4: Commit**

```bash
git add plugins/job-hunter/skills/application-packager/SKILL.md
git commit -m "feat(job-hunter): reuse saved CV in application-packager instead of re-asking"
```

---

### Task 2: Add CV load/save flow to `cover-letter-writer`

**Files:**
- Modify: `plugins/job-hunter/skills/cover-letter-writer/SKILL.md`

- [ ] **Step 1: Replace the CV input step**

Find this exact text:
```markdown
1. "Please paste your CV below." (Wait for response.)
```

Replace it with:
```markdown
1. Check whether `./job-search-tracker/cv.md` exists and is non-empty.
   - If found: say "Found your saved CV — using it." and use its contents. Do not ask the user to paste anything — skip to the next input.
   - If not found: ask "Please paste your CV below." (Wait for response.) Once given, save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist).
   - If the user pastes a new CV unprompted later even though a saved one was used, overwrite `cv.md` with the new content immediately — no confirmation needed.
```

- [ ] **Step 2: Verify the file reads cleanly**

Run:
```bash
grep -n "cv.md" plugins/job-hunter/skills/cover-letter-writer/SKILL.md
```
Expected: 3 matching lines.

- [ ] **Step 3: Commit**

```bash
git add plugins/job-hunter/skills/cover-letter-writer/SKILL.md
git commit -m "feat(job-hunter): reuse saved CV in cover-letter-writer instead of re-asking"
```

---

### Task 3: Add CV load/save flow to `fit-analyzer`

**Files:**
- Modify: `plugins/job-hunter/skills/fit-analyzer/SKILL.md`

- [ ] **Step 1: Replace the CV input step**

Find this exact text:
```markdown
1. "Please paste your CV below." (Wait for response.)
```

Replace it with:
```markdown
1. Check whether `./job-search-tracker/cv.md` exists and is non-empty.
   - If found: say "Found your saved CV — using it." and use its contents. Do not ask the user to paste anything — skip to the next input.
   - If not found: ask "Please paste your CV below." (Wait for response.) Once given, save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist).
   - If the user pastes a new CV unprompted later even though a saved one was used, overwrite `cv.md` with the new content immediately — no confirmation needed.
```

- [ ] **Step 2: Verify the file reads cleanly**

Run:
```bash
grep -n "cv.md" plugins/job-hunter/skills/fit-analyzer/SKILL.md
```
Expected: 3 matching lines.

- [ ] **Step 3: Commit**

```bash
git add plugins/job-hunter/skills/fit-analyzer/SKILL.md
git commit -m "feat(job-hunter): reuse saved CV in fit-analyzer instead of re-asking"
```

---

### Task 4: Add CV load/save flow to `cv-tailor`

**Files:**
- Modify: `plugins/job-hunter/skills/cv-tailor/SKILL.md`

- [ ] **Step 1: Replace the CV input step**

Find this exact text:
```markdown
1. "Please paste your CV below." (Wait for response.)
```

Replace it with:
```markdown
1. Check whether `./job-search-tracker/cv.md` exists and is non-empty.
   - If found: say "Found your saved CV — using it." and use its contents. Do not ask the user to paste anything — skip to the next input.
   - If not found: ask "Please paste your CV below." (Wait for response.) Once given, save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist).
   - If the user pastes a new CV unprompted later even though a saved one was used, overwrite `cv.md` with the new content immediately — no confirmation needed.
```

- [ ] **Step 2: Add a rule against overwriting cv.md with the tailored output**

Find this exact text:
```markdown
**Hard constraints — never:**
- Invent experiences, tools, results, or metrics not in the source CV
- Change job titles, dates, or company names
- Add quantitative claims not in the original
- Inflate scope beyond what the CV states
```

Replace it with:
```markdown
**Hard constraints — never:**
- Invent experiences, tools, results, or metrics not in the source CV
- Change job titles, dates, or company names
- Add quantitative claims not in the original
- Inflate scope beyond what the CV states
- Overwrite `./job-search-tracker/cv.md` with this tailored output — only the original source CV loaded or pasted in Step 1 belongs there
```

- [ ] **Step 3: Verify the file reads cleanly**

Run:
```bash
grep -n "cv.md" plugins/job-hunter/skills/cv-tailor/SKILL.md
```
Expected: 3 matching lines.

- [ ] **Step 4: Commit**

```bash
git add plugins/job-hunter/skills/cv-tailor/SKILL.md
git commit -m "feat(job-hunter): reuse saved CV in cv-tailor instead of re-asking"
```

---

### Task 5: Add CV load/save flow to `interview-prep`

**Files:**
- Modify: `plugins/job-hunter/skills/interview-prep/SKILL.md`

- [ ] **Step 1: Replace the CV input step**

Find this exact text:
```markdown
2. "Please paste your CV below." (Wait for response.)
```

Replace it with:
```markdown
2. Check whether `./job-search-tracker/cv.md` exists and is non-empty.
   - If found: say "Found your saved CV — using it." and use its contents. Do not ask the user to paste anything — skip to the next input.
   - If not found: ask "Please paste your CV below." (Wait for response.) Once given, save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist).
   - If the user pastes a new CV unprompted later even though a saved one was used, overwrite `cv.md` with the new content immediately — no confirmation needed.
```

- [ ] **Step 2: Verify the file reads cleanly**

Run:
```bash
grep -n "cv.md" plugins/job-hunter/skills/interview-prep/SKILL.md
```
Expected: 3 matching lines.

- [ ] **Step 3: Commit**

```bash
git add plugins/job-hunter/skills/interview-prep/SKILL.md
git commit -m "feat(job-hunter): reuse saved CV in interview-prep instead of re-asking"
```

---

### Task 6: Save `cv-builder`'s generated CV to the shared location

**Files:**
- Modify: `plugins/job-hunter/skills/cv-builder/SKILL.md`

- [ ] **Step 1: Add the save step at the end of the Composition Phase**

The file currently ends with this exact text (note the final line is the closing ``` of the "Output format" code block — that's the literal last line of the file):

````
## Education
**[Degree]**, [Institution] — [MMM YYYY]

## Skills
**[Category]:** skill1, skill2, skill3
**[Category]:** skill1, skill2
```
````

Find that exact text (ending in the closing ``` of the code block) and replace it with the same text plus this new paragraph appended after the closing ```:

````
## Education
**[Degree]**, [Institution] — [MMM YYYY]

## Skills
**[Category]:** skill1, skill2, skill3
**[Category]:** skill1, skill2
```

After presenting the composed CV to the user, also save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist), overwriting any previously saved CV. This makes it the shared source CV for the other job-hunter skills, so the user won't be asked to paste it again.
````

- [ ] **Step 2: Verify the file reads cleanly**

Run:
```bash
grep -n "cv.md" plugins/job-hunter/skills/cv-builder/SKILL.md
```
Expected: 1 matching line.

- [ ] **Step 3: Commit**

```bash
git add plugins/job-hunter/skills/cv-builder/SKILL.md
git commit -m "feat(job-hunter): save cv-builder output to shared CV location"
```

---

### Task 7: Manual validation

No automated test framework — this is a manual smoke test.

**Files:** none (validation only)

- [ ] **Step 1: Validate first-run save**

In a scratch directory with no `job-search-tracker/` folder, run `fit-analyzer` (or any of the 5 CV-consuming skills) with a sample CV pasted when asked.

Expected: the skill asks to paste the CV as before, and after pasting, `./job-search-tracker/cv.md` is created containing exactly the pasted content.

- [ ] **Step 2: Validate reuse on second run**

In the same directory, run a *different* one of the 5 skills (e.g. `cv-tailor` if step 1 used `fit-analyzer`).

Expected: the skill says something like "Found your saved CV — using it," does not ask to paste, and uses the previously saved content correctly.

- [ ] **Step 3: Validate cv-builder writes to the same location**

In a fresh scratch directory, run `cv-builder` through a full interview and let it compose the final CV.

Expected: after the CV is presented, `./job-search-tracker/cv.md` is created (or overwritten) with that generated CV's content, and a subsequent run of `fit-analyzer` in the same directory picks it up without asking to paste.

- [ ] **Step 4: Validate overwrite-on-new-paste**

With a saved CV already present, run any of the 5 consuming skills again but paste a different CV instead of accepting the saved one implicitly (simulate by explicitly telling the skill "here's an updated CV" with new content when it announces the saved one).

Expected: `cv.md` is overwritten with the new content, no confirmation prompt blocks it.

- [ ] **Step 5: Record validation result**

Note whether all four expectations held. If anything diverged, fix the relevant SKILL.md from Tasks 1–6 and re-validate before considering the plan complete.

---

## Self-review notes

- **Spec coverage:** Tasks 1–5 cover the check-then-load-or-save flow for all 5 CV-consuming skills listed in the spec; Task 6 covers the `cv-builder` integration; the "what is NOT cached" rules (JD, cv-tailor's tailored output) are covered by Task 4 Step 2 and the spec's existing silence on JD caching (no JD-caching code was added anywhere, which is correct — absence is the implementation of that non-goal). Task 7 covers the spec's validation approach.
- **Non-goals respected:** no new skill/shared component was created — each edit is inline in its own file, consistent with the spec's stated approach; no versioning/history added; no confirmation-before-overwrite added.
- **Consistency:** the load/save instruction text is identical (modulo step numbering and each file's existing "(Wait...)" phrasing) across Tasks 1–5, and all reference the same path `./job-search-tracker/cv.md` used by `notion-connector`.
