---
name: role-calibration
description: Converts ambiguous hiring requests, job descriptions, and hiring-manager candidate feedback into structured recruiting plans, search criteria, and scoring rubrics.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Role Calibration

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user asks to define, calibrate, unpack, or turn a role, job description, intake note, Slack message, or hiring request into reusable sourcing criteria. Also use it when the user wants to update an existing role plan from hiring-manager or teammate feedback on a shortlist.

## Workflow

1. If the user provides a URL, call web fetch before using the content.
2. For a broad hiring request, clarify missing goals, role/profile, level, geography/remote scope, or essential experience that would materially change the plan. Use one focused batch in your reply and wait for the answers. Reuse the conversation and supplied brief; do not re-ask known details. If the user explicitly asks for a first draft or tells you to proceed with assumptions, draft immediately and mark missing information as Unknown.
3. Package the plan into a document (your own document or artifact tool), passing the complete structured markdown. It must be reusable for search, scoring, outreach, and hiring-manager review.
4. Do not create a project on your own. If this chat is not already linked to a project, offer to track the role as a project; only call a SourceGeek project (created in the SourceGeek app) (using the role title or a concise name derived from it) when the user asks for it or accepts the offer.
5. After the artifact is created, mention only remaining open decisions that materially change sourcing; do not repeat the initial questions or start another intake round for minor details.
6. Do not run `fast_search_leads` or `score_leads` unless the user explicitly asks to source or score candidates.

## Artifact Structure

The artifact must include these sections in order:

- Role snapshot
- Confirmed requirements
- Inferred assumptions
- Search strategy
- Candidate scoring rubric
- Target company profiles
- Boolean search concepts
- Open hiring-manager questions
- Recommended first sourcing run
- Overconstraint or market-size warnings

## Feedback Calibration

When the user asks to process shortlist review feedback (or asks what the hiring team said), run this loop:

1. Call `read_shortlist_feedback` for the shortlist artifact. Summarize the feedback per candidate with attribution: decision, signals, and comments, naming each reviewer.
2. Extract reusable signals, not one-off reactions. "Too junior" on two candidates becomes a seniority floor; "find more like candidate 2" becomes a search strategy built from that candidate's title, company type, domain, and trajectory (read the dossier with `read_artifact` for the evidence).
3. Propose rubric updates as an explicit list of changes to the calibration artifact — each with the feedback that motivated it. Never silently rewrite criteria: apply the changes with your document tools or your document tools only after the user approves them.
4. Detect contradictions (one reviewer advances what another rejects for a structural reason, or feedback conflicts with a confirmed requirement). Surface the conflict and ask which way to calibrate instead of picking a side.
5. Refuse to encode feedback about protected characteristics — age, gender, family status, nationality, ethnicity, religion, disability, or similar — into search criteria or scoring weights. Flag that the signal was excluded and why.

## ESCO Taxonomy Grounding

For vague, niche, or local-language titles, ground the plan in the EU's ESCO taxonomy (free, no connector needed): `esco_search` the title (in the user's language when they write in one), then `esco_occupation` on the best match. Use its alternative labels for the Boolean search concepts and title variants, its broader/narrower occupations to spot adjacent talent pools, and its essential/optional skills as a checklist for open hiring-manager questions. ESCO describes the occupation in general — list its skills as inferred assumptions, never as confirmed requirements.

## Market Reality Check (Intelligence Group, when connected)

When the Intelligence Group connector is connected, ground the overconstraint and market-size warnings in data instead of intuition:

- Resolve the role with `resolve_occupation` from the user's intelligence-group connector (if connected), then call `get_market_scarcity` from the user's intelligence-group connector (if connected) for the target group size (nationally and per region), the recruitment feasibility score, and how actively the group is looking for work. Pass `region` when the role is location-bound.
- Quote the feasibility score and target group size in the "Overconstraint or market-size warnings" section, and say plainly what it implies for time-to-hire — a very scarce profile plus a strict location is a plan the hiring manager should see the cost of up front.
- `get_target_group_profile` from the user's intelligence-group connector (if connected) checks whether the role's hours and commute are realistic for this market; `get_candidate_motivation` from the user's intelligence-group connector (if connected) shows what this group actually negotiates, which belongs in the open hiring-manager questions when the brief is silent on it.

If the connector is not connected, write the warnings from search-side reasoning as usual and mark them as estimates.

For Dutch roles, CBS open data (free, always available) adds official context either way: `cbs_search_tables` for "vacatures beroep" or "openstaande vacatures", then `cbs_table_info` and `cbs_query_data` for the latest vacancy counts of the role's occupation group or sector in its region. Cite the table as "CBS StatLine, table <id>" and note that occupation groups are broader than the job title.

## Calibration Guidance

Separate confirmed requirements from inferred assumptions. Never invent compensation, visa support, relocation policy, willingness to hire remote, interview process details, or company policy.

Convert vague requirements into observable candidate signals. For example, rewrite "strong communication" as evidence such as customer-facing technical work, design docs, mentoring, public writing, or cross-functional project ownership.

Flag searches that may be too narrow, such as strict geography plus rare skills plus narrow domain plus seniority constraints. Recommend a broader first pass when the likely market is small.
