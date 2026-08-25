---
name: cover-letter-writer
description: Writes a targeted, specific cover letter for a job application using company research and fit analysis. Use when someone asks to write a cover letter, draft a cover letter, or create an application letter for a specific job.
---

# Cover Letter Writer

Write a cover letter that opens with a specific company hook — never a generic "I'm excited" opener.

## Instructions

Ask for inputs one at a time:

1. Check whether `./job-search-tracker/cv.md` exists and is non-empty.
   - If found: say "Found your saved CV — using it." and use its contents. Do not ask the user to paste anything — skip to the next input.
   - If not found: ask "Please paste your CV below." (Wait for response.) Once given, save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist).
   - If the user pastes a new CV unprompted later even though a saved one was used, overwrite `cv.md` with the new content immediately — no confirmation needed.

2. "Now paste the job description." (Wait for response.)

3. Run 3 quick web searches on the company:
   - "[Company] news [current year]"
   - "[Company] product mission recent"
   - "[Company] [role] culture values"

4. Identify the 2 strongest overlaps between the CV and JD.

5. Write a cover letter that:
   - Opens with a SPECIFIC hook tied to company research — a product launch, a strategic shift, a leader's quote, a specific product the candidate knows — NOT "I'm excited about your mission" or "I'm writing to express my interest"
   - Is assertive and specific — no generic language
   - Leads with the 2 strongest overlaps from the fit
   - Acknowledges a significant gap only if it's obvious and would be damaging to leave unaddressed
   - Closes confidently with one clear next step
   - Is exactly 3 short paragraphs
   - Is under 280 words total — cut ruthlessly

6. Format as plain text with Dear / Sincerely structure.
   Do not include date or address block (the user adds those).

## Rules
- The opener must be specific to this company — if you can't find anything in search, use the most specific JD signal instead
- If web search returns nothing useful, proceed with CV + JD alone and briefly note that research was limited
- Specific beats impressive — cite the actual thing you found, not a paraphrase
- Never use: "I am excited", "I am writing to express", "I would be a great fit", "passionate about"
