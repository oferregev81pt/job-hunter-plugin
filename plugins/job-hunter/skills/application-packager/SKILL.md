---
name: application-packager
description: Builds a complete job application package in one guided conversation: company research, fit analysis, tailored CV, cover letter, and stage prep doc. Use when someone asks to build their full application, give me everything I need to apply, do full application prep, or wants one session to get completely ready for a specific job.
---

# Application Packager

Guide the user through a complete application package in one conversation. Collect all inputs once, then generate every deliverable.

## Instructions

### Step 1: Collect inputs (once — reuse across all steps)

Ask one at a time:

1. Check whether `./job-search-tracker/cv.md` exists and is non-empty.
   - If found: say "Found your saved CV — using it." and use its contents. Do not ask the user to paste anything — skip to the next input.
   - If not found: ask "Please paste your CV below." (Wait.) Once given, save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist).
   - If the user pastes a new CV unprompted later even though a saved one was used, overwrite `cv.md` with the new content immediately — no confirmation needed.
2. "Now paste the job description — or share the URL and I'll fetch it." (Wait.)
3. "Which experience from your background do you most want to highlight for this role?" (Wait.)
4. "Any gap or weakness you're concerned about?" (Wait.)

### Step 2: Company Research

Say: "Let me research [Company] now."

Run 3 web searches:
- "[Company] news [current year]"
- "[Company] culture leadership values"
- "[Company] [role] interview Glassdoor"

Summarize in ~150 words: what they do, 2–3 recent news items with sources, 2–3 things to reference in conversation.

### Step 3: Fit Analysis

Silently run an honest fit assessment:
- Identify 3–5 strong matches (JD requirement ↔ CV evidence with specifics)
- Identify 2–3 partial matches with reframe suggestions
- Identify 1–2 honest gaps with recommendations
- State a fit verdict: strong / stretch / long_shot

### Step 4: Tailored CV

Rewrite the CV for this specific role:
- Tailored summary paragraph
- Bullets reordered within each job (most relevant to JD first)
- Bullet wording updated to surface JD keywords where genuine match exists
- Skills section reordered
- Never invent metrics, titles, dates, or employers not in the original

### Step 5: Cover Letter

Write a 3-paragraph cover letter under 280 words:
- Paragraph 1: Specific company hook from research (product, news, strategic shift) — never "I am excited to apply"
- Paragraph 2: The 2 strongest overlaps between CV and JD
- Paragraph 3: Confident close with one clear next step

Format: plain text, Dear / Sincerely structure, no date or address block.

### Step 6: Application Prep

Generate an applying-now prep document:
- One-line fit verdict with brief reasoning
- 3-bullet pitch (the strongest overlaps)
- Any gaps to address in the cover letter or ignore
- Application checklist: CV ✓ / cover letter ✓ / submitted ✓ / tracked ✓

### Step 7: Deliver everything

Output as one consolidated document:

---

# Application Package: [Role] @ [Company]

## Company Research
[~150 word summary with sources]

## Fit Analysis
### Strong matches
- [requirement] ↔ [CV evidence]

### Gaps
- [gap] — recommendation

### Verdict
[strong | stretch | long_shot] — [brief reasoning]

## Tailored CV
[full CV in markdown]

## Cover Letter
[full letter, plain text]

## Application Checklist
- [ ] CV reviewed and exported
- [ ] Cover letter personalized
- [ ] Application submitted via [portal/email/LinkedIn from JD]
- [ ] Tracked: [Company] | [Role] | Applied [date]

---

## Rules
- Collect all inputs in Step 1 before generating anything
- Never invent CV content — no fabricated metrics, titles, or employers
- The cover letter opener must be specific to this company, never generic
- If web search yields nothing, note it and proceed from CV + JD alone
- Keep the full output as one document so the user can copy it all at once
- Never overwrite `./job-search-tracker/cv.md` with the tailored CV generated in Step 4 — only the original source CV loaded or pasted in Step 1 belongs there
