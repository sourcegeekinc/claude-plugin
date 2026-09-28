---
name: company-intel
description: Builds citation-backed company dossiers for pre-pitch prep, candidate briefings, and hiring-manager context.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Company Intel

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user needs company context before pitching a client, briefing a candidate on an employer, or preparing for a hiring-manager conversation.

## Workflow

1. Confirm the company name, URL, and what the dossier is for (client pitch, candidate briefing, competitive context).
2. For quick structured facts (headcount, funding, founding year, HQ, key leaders), use `enrich_entity` with explicit fields — citation-backed, no hallucination.
3. For single follow-up questions ("who is their VP Engineering?"), use web search.
4. When the user provides a company URL or job posting URL, use web fetch to read it before synthesizing.
5. For a full dossier (product, culture signals, recent news, engineering blog presence, hiring themes), use in-depth web research. It returns the report as cited markdown — it does not save anything on its own.
6. Package the dossier into a document (your own document or artifact tool) (pass the returned markdown, or your own synthesized markdown): company snapshot, key facts, culture/hiring signals, recent news, risks, and suggested talking points. Do this whenever the user wants a reusable summary.

## LinkedIn Company Signals

For the company's own LinkedIn facts — headcount and range, follower count, HQ, recent company posts, open job count and postings, the recruiter on a posting, and (when growth or hiring trends matter) the premium insights — use the `linkedin_company_*` and `linkedin_job_*` tools as described in the `linkedin-intelligence` skill. Cite them as "LinkedIn" with the fetch date.

## Latency Warning

Before calling in-depth web research, warn the user it takes several minutes. If the run exceeds the time budget, surface the run id and partial status so the user knows research may still be in progress.

## Provider Routing

- Use `enrich_entity` for specific structured fields about a known company or person — fast, citation-backed.
- Use web search for one-off factual questions.
- Use in-depth web research for comprehensive company diligence with cited sources.
- Prefer web research over in-depth web research only when structured JSON output or index-backed sources (papers, news archives) are specifically needed — it completes synchronously in under a minute.

## Dutch Companies (KVK Handelsregister)

When the KVK connector is connected and the company is Dutch (or the user asks to verify registry facts), ground the dossier in the official business register instead of relying on web estimates:

1. Resolve the company with `search_companies` from the user's kvk connector (if connected) (by name, or KVK number if known) — trading names can differ from statutory names, so confirm the match with `get_trade_names` from the user's kvk connector (if connected) when ambiguous.
2. Pull authoritative facts with `get_company_profile` from the user's kvk connector (if connected): statutory name, legal form, SBI activity codes, registration date, addresses, and total employees. Use `get_company_owner` from the user's kvk connector (if connected) to surface the legal entity behind a trading company, and `list_establishments` from the user's kvk connector (if connected) / `get_establishment_profile` from the user's kvk connector (if connected) for the branch network and per-location headcount.
3. In the dossier, cite registry facts as "KVK registry" and include the KVK number. Where web research and the registry disagree (headcount, founding year, entity names), flag the discrepancy — the registry wins on legal facts; web sources win on recency of commercial signals.

If the KVK connector is not connected, say so when Dutch registry verification would help, and continue with web-research-based facts clearly labeled as unverified.

## Employer Signals (Indeed)

When the Indeed connector is connected, enrich the "culture/hiring signals" section with the employer's Indeed data via its `indeed__` tools: the company's review and salary insights, and a search of its current open postings for hiring volume and the roles it is investing in. Cite these as "Indeed" with the date pulled; reviews are self-reported by employees, so present them as sentiment signals, not facts. Skip silently when Indeed is not connected.

## Evidence Discipline

- Separate confirmed facts (with citations) from inference.
- Never invent funding rounds, headcount, leadership names, or culture claims without source backing.
