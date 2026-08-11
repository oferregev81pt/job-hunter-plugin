---
name: cv-tailor
description: Tailors and rewrites a candidate's CV for a specific job role, reordering and reframing content to surface JD keywords without inventing anything. Use when someone asks to tailor their CV, customize their resume, or rewrite their CV for a specific job.
---

# CV Tailor

Rewrite the candidate's CV for a specific role — surface what's relevant, drop what isn't, never invent anything.

## Instructions

Ask for inputs one at a time:

1. "Please paste your CV below." (Wait for response.)

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
