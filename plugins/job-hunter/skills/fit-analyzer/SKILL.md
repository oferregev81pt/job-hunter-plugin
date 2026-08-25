---
name: fit-analyzer
description: Provides an honest fit assessment between a candidate's CV and a job description. Use when someone asks "am I a good fit?", "should I apply?", "honest fit check", or wants to know how their background matches a specific role.
---

# Fit Analyzer

Give an honest, evidence-based fit assessment — not a pep talk.

## Instructions

Ask for inputs one at a time:

1. Check whether `./job-search-tracker/cv.md` exists and is non-empty.
   - If found: say "Found your saved CV — using it." and use its contents. Do not ask the user to paste anything — skip to the next input.
   - If not found: ask "Please paste your CV below." (Wait for response.) Once given, save it verbatim to `./job-search-tracker/cv.md` (creating the `job-search-tracker/` folder first if it doesn't exist).
   - If the user pastes a new CV unprompted later even though a saved one was used, overwrite `cv.md` with the new content immediately — no confirmation needed.

2. "Now paste the job description — or share the URL and I'll fetch it." (Wait for response.)

3. Silently extract role essentials from the JD:
   - Company name and exact role title
   - Top 5 responsibilities
   - Must-have skills/experience
   - Nice-to-haves
   - Cultural signals
   - Anything unusual

4. Ask three framing questions, one at a time. Generate the options from their actual CV content:

   Q1: "Which of your experiences would you most want to highlight for this role?"
   Generate 4 specific options drawn from their CV (mention actual companies, roles, technologies), plus "Other / let me describe".

   Q2: "Which gap or weak spot are you most worried about?"
   Generate 3 gaps you identified by comparing JD requirements to CV evidence, plus "No major gaps — I feel like a strong fit".

   Q3: "What special intel do you have about [Company]?"
   Options: "I've talked to someone who works there" / "I know their product well / use it myself" / "I know about a specific challenge they're facing" / "I've seen their content (YouTube, blog, talks)" / "No special intel beyond the JD"

5. Return this exact markdown:

# Fit Analysis: [Role] @ [Company]

## Strong matches
- [JD requirement] ↔ [specific CV evidence with company/dates]
(List 3–6 genuine matches)

## Partial matches / reframes needed
- [JD item] — adjacent experience: [...]. Will frame as: [...]
(List 2–4 items)

## Honest gaps
- [JD item] — no direct evidence. Recommendation: [address in cover letter / acknowledge / ignore]
(List 1–3 honest gaps)

## Overall read
[2–3 sentences. Realistic — not a pep talk. Include a fit verdict: Strong fit / Reasonable stretch / Long shot. Say WHY.]

## Fit verdict
[Exactly one of: strong | stretch | long_shot]

## Rules
- Use specific CV evidence — actual company names, dates, technologies from their CV
- Do not inflate or soften the assessment
- Treat the user as a senior professional who can handle honesty
- The framing questions must use options specific to their CV, not generic placeholders
