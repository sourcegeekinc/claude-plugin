---
name: candidate-deep-profile
description: Researches a candidate's public footprint beyond LinkedIn — talks, blog posts, open source, papers — for evidence-backed outreach personalization.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Candidate Deep Profile

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user wants to understand what a candidate has written, spoken about, or built publicly — especially to personalize outreach or differentiate senior candidates beyond LinkedIn.

## Workflow

1. Identify the candidate from chat (name + company/title) or from a shortlist/sheet via `read_artifact`.
2. Use web search with category `people` or `personal site` to find personal sites, conference talks, open-source repos, publications, and interviews.
3. Use web fetch to read the best hits. Summarize verified public footprint only — clearly separate confirmed evidence from inference.
4. When the user provides a reference profile URL, use web search for similar pages to find people in the same niche at similar seniority.
5. Use web search for single follow-up factual questions (e.g. "what conference did they speak at in 2025?").
6. Feed findings into outreach personalization. Pair with the `outreach-writing` skill when the user wants draft messages. When the user wants a reusable profile, package the findings into a document (your own document or artifact tool).

## Provider Routing

- Prefer your web search and fetch tools for people discovery and reading public content. These are fast (seconds).
- Use `get_linkedin_contact_details` only when the user explicitly needs LinkedIn profile enrichment, not for broad public-footprint research.
- For the candidate's LinkedIn activity (posts, comments, reactions, articles, recommendations, interests) use the `linkedin_profile_*` tools via the `linkedin-intelligence` skill — they are the source for what the person actually says on LinkedIn.
- Do not use in-depth web research, web research, or `find_all_entities` for single-candidate profiles — they take minutes and are overkill.

## Evidence Discipline

- Label confirmed public evidence, inferred fit, and missing information separately.
- Never invent employment history, contact details, publications, or willingness to move.
- Do not use protected-class reasoning.
