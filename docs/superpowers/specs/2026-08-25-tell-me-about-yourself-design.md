# Design: "Tell me about yourself" section for interview-prep

## Context

The `interview-prep` skill (`plugins/job-hunter/skills/interview-prep/SKILL.md`)
generates stage-specific prep documents. Currently the HR/recruiter screen
stage template only has a one-line placeholder for this question:

```
- "Tell me about yourself" — [which thread from their CV to lead with for this role]
```

This just points at which CV thread to use — it doesn't produce an actual
answer. The user wants a fully scripted answer generated, since this is one
of the most common and highest-leverage interview questions.

## Change

Add a full "Tell me about yourself" script to both the **HR / recruiter
screen** and **Professional / hiring manager interview** stage templates in
`interview-prep/SKILL.md`.

### Format: Present → Past → Future, full first-person script

```markdown
## Tell me about yourself
[Full first-person spoken answer, 60–90 seconds, ready to read near-verbatim:
- Present: current role/title and the specific slice of it most relevant to the target role
- Past: 1–2 career beats that build the throughline to this role, drawn from actual CV bullets
- Future: why this specific role/company is the logical next step, tied to the JD]
```

### Content rules
- Must pull specific facts (titles, companies, concrete achievements) from
  the pasted CV — never generic filler ("I'm a passionate professional...").
- Must land on a concrete bridge to the JD's core requirement, not a vague
  "I'm excited about this opportunity."
- Both stages use the same instruction text (not two divergent specs) — each
  stage document is generated independently, so a slightly different phrasing
  may appear across the two if regenerated separately, which is fine and
  consistent with how the rest of the skill already works (e.g. "Why
  [Company]?" appears independently in both stage templates today).

### Placement
- **HR / recruiter screen** stage: replace the existing bullet
  `- "Tell me about yourself" — [which thread from their CV to lead with for this role]`
  under "Expected HR questions and your angles" with the full script,
  presented as its own `## Tell me about yourself` subsection placed directly
  above "Expected HR questions and your angles" (so the full answer comes
  first, and the shorter bullet list of other expected questions follows).
- **Professional / hiring manager interview** stage: add a new
  `## Tell me about yourself` subsection, placed after "Stories to bring
  (STAR format)" and before "Likely questions and angles" (the full script
  reads naturally as a bridge between the prepared stories and the other
  anticipated questions).

## Non-goals
- No change to the "Applying now" or "Home assignment" stage templates —
  this question isn't typically asked at those stages.
- No new user-facing question added to the input-collection step (Step 1–4
  of the skill) — the script is generated from the CV/JD/research already
  collected.

## Testing / validation approach

Prompt-only change, no automated tests. Validate by running `interview-prep`
end-to-end for both the HR screen and professional interview stages with a
sample CV/JD and confirming: the generated "Tell me about yourself" section
references specific, real details from the sample CV (not generic phrasing),
follows the Present→Past→Future shape, and lands on a specific bridge to the
sample JD.
