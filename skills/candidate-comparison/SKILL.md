---
name: candidate-comparison
description: Compares 2–5 selected candidates side by side against the role rubric — evidence, risks, missing data, and a decision recommendation per candidate. Use when the user asks to compare specific candidates, decide between finalists, or wants a comparison matrix.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Candidate Comparison

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user selects specific candidates and asks to compare them, choose between them, or decide who to advance. For scoring or ranking a whole sheet use lead-scoring; for hiring-manager one-pagers use candidate-dossier.

## Workflow

1. Identify the candidates to compare — 2 to 5, named by the user or selected from a sheet, shortlist, or dossier artifact. Read their data with `read_artifact`, passing the explicit artifact id. If the user wants more than 5 compared, ask them to narrow the set, or suggest scoring the whole sheet first (lead-scoring) and comparing the top few.
2. Identify the role criteria. Prefer the role-calibration artifact for this role (read it with `read_artifact`); otherwise use the job description or criteria the user provided. The comparison dimensions come from that rubric — must-haves, nice-to-haves, seniority, location and availability constraints — never from a generic attribute list. If no rubric exists, state the criteria you are assuming and invite correction.
3. Gather the evidence per candidate: sheet row data, score metadata if the sheet was scored with `score_leads` (if it was not, offer to score it first, passing the sheet's artifact id), dossier content, and hiring-manager feedback via `read_shortlist_feedback` when the candidates appear on a reviewed shortlist. Attribute feedback to its reviewer — it is an opinion on record, not a confirmed fact about the candidate.
4. Build the comparison as a document (your own document or artifact tool):
   - **Rubric matrix** — one row per criterion (must-haves first), one column per candidate. Each cell states met / not met / unknown, with the evidence behind it. Unknown means unknown: name it as missing evidence rather than guessing.
   - **Per-candidate card** — current role and company relevance, strengths, risks, missing evidence, outreach angle grounded in confirmed evidence, and a decision label (see below) with a recommended next step.
   - **Summary** — a "best for" line and a "main concern" line per candidate, so a hiring manager can read the decision surface in ten seconds.
5. Present the decision summary in the reply itself: each candidate's label, the one-line reason, and what missing evidence would change the call. Then point to the artifact for the full matrix. "Created the comparison, see the artifact" alone is not a decision-ready answer.

## Decision Labels

Use exactly these labels so comparisons stay consistent across chats:

- **Strong advance**
- **Advance with concern**
- **Needs more evidence**
- **Maybe for another role**
- **Reject**

## Evidence Discipline

- Compare evidence, not vibes: every matrix cell and every strength or risk cites sheet data, score metadata, dossier content, or attributed feedback.
- Label confirmed evidence, inferred fit, and missing information separately — never present the three as equally certain.
- Do not force a ranking the evidence cannot support. When candidates are not comparable on a criterion because the data is missing, say so and use "Needs more evidence" instead of inventing a winner.
- Do not fabricate employment history, education, skills, contact details, candidate intent, or willingness to move.
- Compare only on job-related rubric fields. Never compare on protected traits — age, gender, ethnicity, religion, family status, nationality, disability, or similar — and if the user or recorded feedback asks for that, refuse the criterion, say why, and continue with the job-related ones.

## Recommendation Guidance

End with concrete next steps per candidate: advance to which stage, what to verify for "needs more evidence" (and which tool or source would verify it), and what to say in outreach. Recommendations that would change with one piece of missing data should say which piece.
