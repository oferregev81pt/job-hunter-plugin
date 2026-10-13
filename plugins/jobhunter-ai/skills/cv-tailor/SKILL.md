---
name: cv-tailor
description: Tailors and rewrites a candidate's CV for a specific job role, reordering and reframing content to surface JD keywords without inventing anything. Use when someone asks to tailor their CV, customize their resume, or rewrite their CV for a specific job.
---

# CV Tailor

Rewrite the candidate's CV for a specific role — surface what's relevant, drop what isn't, never invent anything.

## Instructions

Ask for inputs one at a time:

1. Check whether `./job-search-tracker/cv.md` exists and is non-empty.
   - If found: say "Found your saved CV — using it." and use its contents. Do not ask the user to paste anything — skip to the next input.
   - If not found: ask "Please paste your CV below." (Wait for response.) Once given, save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist).
   - If the user pastes a new CV unprompted later even though a saved one was used, overwrite `cv.md` with the new content immediately — no confirmation needed.

2. "Now paste the job description." (Wait for response.)

3. "Which experience or thread from your background do you most want to highlight for this role?" (Wait for response.)

4. "Is there any gap or weak spot you're especially concerned about?" (Wait for response.)

5. Rewrite the full CV following these rules:

**What to change:**
- Rewrite the summary paragraph to be specific to this role
- Reorder bullets within each job — most relevant to this JD first
- Rewrite bullet wording to surface JD keywords where the underlying work genuinely matches
- Adjust the skills section to lead with role-relevant items
- Drop or compress clearly irrelevant bullets (do not delete entire jobs)

**Hard constraints — never:**
- Invent experiences, tools, results, or metrics not in the source CV
- Change job titles, dates, or company names
- Add quantitative claims not in the original
- Inflate scope beyond what the CV states
- Overwrite `./job-search-tracker/cv.md` with this tailored output — only the original source CV loaded or pasted in Step 1 belongs there

6. Return the full tailored CV as clean markdown:

# [Full Name]
[Contact info]

## Summary
[Tailored for this specific role — 2–3 sentences]

## Experience
### [Job Title] | [Company] | [Dates]
- [bullets, most relevant to this JD first]

## Education
### [Degree] | [Institution] | [Year]

## Skills
[Categories with role-relevant items first]
