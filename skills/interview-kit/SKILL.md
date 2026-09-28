---
name: interview-kit
description: Builds structured interview kits — competency matrices, question banks, scorecards, interviewer briefs — from role requirements. Use when the user asks for interview questions, scorecards, interview plans, or panel prep.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Interview Kit

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user wants a structured interview plan for a role: what each stage tests, what to ask, and how to score answers consistently.

## Workflow

1. Start from the role-calibration artifact or JD (`read_artifact`); if neither exists, gather the must-have requirements first or suggest running role-calibration.
2. Derive 5–7 competencies from the confirmed requirements. Each competency gets observable signals — what a strong answer demonstrates — not adjectives.
3. Map competencies to stages so nothing is tested twice and nothing critical is untested. State the mapping explicitly; the hiring team should see the coverage at a glance.
4. For each stage, produce: focus competencies, 4–6 questions (behavioral and role-specific, anchored in the candidate's actual claimed experience where a dossier exists), and per-question indicators of strong versus weak answers.
5. Build the scorecard with `create_sheet`: one row per competency and nothing else — no overall-recommendation, culture, or summary rows; the hire/no-hire call belongs in the debrief, not the scorecard. Use a 1–4 scale with labeled anchors and a free-text evidence column. The same scorecard applies to every candidate for the role.
6. Write interviewer briefs with your own document or artifact tool: their stage's focus, questions, scoring anchors, and what the other stages cover so they do not duplicate.
7. Offer to publish the kit where the hiring team lives — Notion, Google Drive, or OneDrive when connected — and to attach it to the role in the ATS (approval-gated).

## Guardrails

- No questions that touch protected characteristics or invite them: family plans, age, health, origin, religion, or proxies for them.
- Questions test the calibrated requirements — if a question tests something outside them, either drop it or surface the gap in the calibration.
- Scoring anchors must be evidence-based and consistent across candidates; never customize the scorecard per candidate.
- Do not invent the company's interview process, stages, or policies — build on what the user or ATS confirms exists.
