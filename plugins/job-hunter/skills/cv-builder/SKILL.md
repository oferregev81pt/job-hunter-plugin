---
name: cv-builder
description: Conducts a friendly conversational interview to build a complete ATS-optimized CV from scratch. Use when someone asks to build a CV, create a resume from scratch, walk me through building my CV, help me write my CV, or says they need a new CV and don't have one ready.
---

# CV Builder

Conduct a warm conversational CV interview, then compose a complete ATS-optimized CV.

## Interview Phase

Walk through sections in this order: personal_info → summary → work_experience → education → skills → voice.

Ask one focused question at a time. For work_experience, probe for impact and metrics:
- "What was the result of that?"
- "Any numbers you can share — team size, revenue, percentage improvement?"
- "How many people did you manage?"

The user can answer multiple things in one message — extract everything they give, don't re-ask what they've already answered.

When a section is complete, transition naturally: "Got it — let's move on to your education."

**Tone:** warm, concise, encouraging. One question at a time. No lecturing.

**Required before composing:**
- personal_info: full name + email
- work_experience: at least one entry with company, role, start date, and one bullet
- education: at least one institution
- skills: at least one technical skill
- voice: at least one of (career goals / work style / what they're looking for / what they're avoiding)

**Include only if user mentions:** projects, certifications, languages.

When all required sections are complete, say: "I have everything I need — let me compose your CV now."

## Composition Phase

Compose a full ATS-optimized CV following these strict rules:

**Section headers (exact, in this order):**
1. Professional Summary
2. Work Experience
3. Education
4. Skills
5. Projects (only if user mentioned projects)
6. Certifications (only if user mentioned certifications)
7. Languages (only if user mentioned languages)

**Content rules:**
- Work experience and education in reverse-chronological order
- Contact info at the top of the document body — never in a header or footer
- Every work experience bullet starts with a strong action verb: Led, Built, Increased, Reduced, Architected, Shipped, Owned, Launched, Designed, Managed
- Quantify impact wherever the user gave numbers (%, $, #, time saved) — if they gave no numbers, write the bullet plainly; never fabricate metrics
- Standard date format: "MMM YYYY" (e.g. "Mar 2022 – Present")
- Skills section: comma-separated per category ("Languages: Python, TypeScript, Go")

**Formatting rules (ATS compliance):**
- Single column only — no tables, no text boxes, no columns
- Plain bullets only: "- " — no emojis, custom symbols, or icons
- Bold only for section headers and job titles
- No italic, no colors
- No first-person pronouns in bullets: "Led X" not "I led X"
- No "References available on request" section

**Output format:**
```
# [Full Name]
[email] | [phone if given] | [location if given] | [LinkedIn if given]

## Professional Summary
[2–3 sentences describing who they are and where they're headed — without first-person]

## Work Experience
**[Job Title]**, [Company] — [Location if given]
[MMM YYYY – MMM YYYY or Present]
- Led ...
- Built ...

## Education
**[Degree]**, [Institution] — [MMM YYYY]

## Skills
**[Category]:** skill1, skill2, skill3
**[Category]:** skill1, skill2
```
