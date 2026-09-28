---
name: jd-authoring
description: Writes posting-ready job descriptions from role requirements or calibration output, with an inclusive-language pass. Use when the user asks to write, draft, improve, or rewrite a JD or job posting.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# JD Authoring

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user wants a job description written or reworked. For comparing an existing JD against competitor postings, use jd-competitive-analysis; this skill produces the posting itself.

## Workflow

1. Start from what exists: a role-calibration artifact (`read_artifact`), a pasted draft, or a conversation. If neither a calibration doc nor clear requirements exist, gather the essentials first — title, level, location and remote policy, top responsibilities, and must-have requirements — or suggest running role-calibration for ambiguous roles.
2. If positioning is unclear, pull one or two comparable postings with web search and web fetch for structure reference — never to copy language from.
3. Draft with your own document or artifact tool in this structure, using these section headings verbatim: title, one-paragraph impact summary (what this person will achieve, not a company ad), "Responsibilities", "Requirements" split into must-haves and nice-to-haves, and practical details (location, remote policy, salary range, process). Recruiters and job boards scan for these canonical headings — do not restyle them ("What you'll do", "Who you are").
4. Keep must-haves honest: when the brief provides a confirmed must-have list, preserve exactly that set of qualifications. Do not add generic screening criteria such as communication, collaboration, ownership, or cultural fit unless they are explicitly in that list. Do not broaden a confirmed domain requirement with alternatives like "or closely related systems". Before saving, check each must-have bullet against the supplied list and remove any addition. Move everything inferred to nice-to-haves or ask. Cut degree requirements unless the user confirms they are genuinely required.
5. Run an inclusive-language pass before presenting: no gendered or age-coded wording ("rockstar", "digital native", "young team"), no idioms that exclude non-native speakers, requirement list short enough not to deter qualified non-traditional applicants.
6. The document is the posting and nothing else. Put edit rationale, change logs, and flagged questions in the chat reply — a "what I changed" section inside the artifact re-introduces the excluded wording and is not postable.
7. Offer to publish or update the posting via the connected ATS when its tools support it. ATS writes require approval, per ats-pipeline-operator conventions.

## Intelligence Group (when connected)

When the Intelligence Group connector is connected, use it to decide what the posting leads with. Resolve the role with `resolve_occupation` from the user's intelligence-group connector (if connected), then call `get_candidate_motivation` from the user's intelligence-group connector (if connected) for the pull factors and job benefits this target group ranks highest, and lead the impact summary and practical details with the ones the role genuinely offers. Never claim a benefit the user has not confirmed — the data says what candidates want, not what this employer provides.

`get_salary_benchmark` from the user's intelligence-group connector (if connected) gives a sourced range to offer the user when recommending salary transparency; present it as market data for the occupation group and let the user confirm the range before it goes in the posting. `get_sourcing_channels` from the user's intelligence-group connector (if connected) tells you where the posting should be distributed — raise it as a follow-up, not as a section in the JD. Skip silently when the connector is not connected.

## ESCO Skills Check

When requirements are thin, look the role up with `esco_search` and `esco_occupation` to check for obvious skill gaps and to pick the standard occupation title job boards and EU matching services (EURES) recognise. Raise the gaps with the user in chat; do not add ESCO skills to the must-haves on your own — the confirmed must-have list rule still applies.

## Guardrails

- Never invent compensation, benefits, visa support, relocation policy, or interview process details. Omit what is unconfirmed, or ask.
- EU/NL postings: do not state requirements that are unlawful to demand (age ranges, nationality beyond work authorization, photos).
- Salary transparency: recommend including a range; if the user declines, comply without repeating the recommendation.
- The JD must be truthful about the role as calibrated — flag mismatches with the calibration doc instead of papering over them.
