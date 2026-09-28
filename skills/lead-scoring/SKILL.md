---
name: lead-scoring
description: Scores, ranks, and explains fit for candidates or contacts. Use when the user asks to score a sheet, prioritize people, rank candidates, evaluate fit, or apply a rubric.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Lead Scoring

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user wants an existing or newly created list evaluated against criteria.

## Workflow

1. Identify the scoring target: job description, role requirements, hiring criteria, or custom rubric.
2. Use `score_leads` on a sheet artifact, passing its artifact id. If the candidates came from a search and are not yet in a sheet, first package them with `create_sheet` (pass the returned `csv` and `searchId`). `score_leads` enriches LinkedIn profiles itself before scoring — the sheet does not need to be pre-enriched.
3. Explain each candidate's score individually, citing the evidence behind it from the sheet, enriched profile metadata, or extracted target text. A collective explanation ("all scored low because…") is not enough — every row in the ranking gets its own one-line evidence trail.
4. Penalize missing must-have criteria, and name the unconfirmed must-haves per candidate as missing evidence. Do not fill gaps with invented facts.
5. After scoring, summarize the highest-confidence next actions and any data that would materially change the ranking.

## Rubric Guidance

Default to a simple 0-100 fit score unless the user provides a rubric. Keep score explanations short, evidence-based, and comparable across rows.
