---
name: candidate-dossier
description: Creates evidence-backed candidate dossiers and shortlists from sourced or scored candidates.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Candidate Dossier

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user asks for a shortlist, top candidates, candidate dossiers, hiring-manager-ready recommendations, or evidence-backed candidate summaries.

## Workflow

1. Identify the scored candidate sheet and active role target. If the target is a URL, call web fetch before scoring.
2. If the sheet is not scored, use `score_leads` with the role target and explicit sheet artifact id.
3. Use `create_shortlist` on the scored sheet to create the shortlist artifact.
4. Present the shortlist in the reply itself: for each shortlisted candidate, a compact summary that separates confirmed evidence from inferred fit from missing information, and flags anyone ranked highly on thin evidence. Then point to the artifact for the full one-pagers and suggest the next recruiting action. "Created the shortlist, see the artifact" alone is not a hiring-manager-ready answer.

## Evidence Discipline

- Use only available sheet data, enriched profile data, and score metadata.
- Label confirmed evidence, inferred fit, and missing information separately.
- Treat must-have and nice-to-have coverage as unknown unless the scored metadata explicitly supports it.
- Highlight high-scoring candidates with weak evidence or missing profile data.
- Do not fabricate employment history, contact details, education, skills, candidate intent, or willingness to move.
- Do not use protected-class reasoning, including age, gender, ethnicity, religion, family status, disability, or other protected traits.

## Recommendation Guidance

Recommend concrete next actions such as advance, maybe, reject for now, or needs more research. Outreach angles must be grounded in confirmed evidence and should avoid generic praise.
