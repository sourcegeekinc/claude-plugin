---
name: recruiting-search
description: Finds and qualifies recruiting candidates. Use when the user asks for candidate sourcing, hiring criteria, role requirements, screening, talent search, recruiters, applicants, or hiring pipelines.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Recruiting Search

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill for recruiting workflows where the user wants to find, qualify, compare, or prioritize candidates.

## Workflow

1. Check the conversation and supplied brief for the role/profile, seniority, geography/remote scope, and essential experience. If the request is broad and missing details would materially change the candidate pool, ask one focused batch in your reply and wait before searching. Do not guess these criteria or re-ask answered questions. Proceed directly for a sufficiently specific brief or an explicit request for an exploratory search with assumptions. Convert the confirmed criteria into title variants, skills, domain experience, geography, and exclusions.
2. Use `fast_search_leads` when the user needs new candidates or people matching criteria. This is the default — it is fast and cheap.
3. Use `precision_search_leads` for hard or high-stakes searches where match quality matters more than speed or cost. It interprets the query more carefully and returns an explanation, but costs more and is rate-limited (~10/hour), so reserve it for when `fast_search_leads` underdelivers.
4. The search tools return the matched candidates as `csv` plus a `searchId`; they do not create a sheet. Package the candidates into a sheet artifact with `create_sheet` (pass both the returned `csv` and `searchId` — the `searchId` attaches the full candidate details shown in the sheet's Details view) so the user can see and work with them. Do this after a search unless the user only wanted a quick count. Keep the LinkedIn column so the sheet can be scored.
5. Only run `score_leads` when the user explicitly asks to score, rank, or prioritize candidates — a detailed search prompt is not such a request. Otherwise, offer scoring as the next step and wait. When asked, pass the sheet's artifact id; `score_leads` enriches LinkedIn profiles itself before scoring — the sheet does not need to be pre-enriched.
6. Separate hard evidence from inferred fit. Never invent candidate history, contact details, or willingness to move.
7. Recommend the next recruiting action: refine search, enrich profiles, score candidates, or draft outreach.
8. For LinkedIn-native sourcing — people who engaged with a hiring post or hashtag, a job's hiring team, look-alikes of one strong profile — load the `employee-intelligence` skill and use the `employee_*` tools.

## Search Query Guidance

Write broad enough search queries to avoid overfitting. Include required criteria first, then nice-to-have criteria. Use alternate titles when roles vary across companies. For alternate titles, including other-language variants, `esco_search` plus `esco_occupation` return the occupation's official alternative labels.

For senior roles, include leadership signals such as team ownership, scale, hiring, architecture, product ownership, or technical ownership only when they matter for the role.
