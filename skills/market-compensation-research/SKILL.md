---
name: market-compensation-research
description: Researches salary benchmarks, talent supply/demand, and market fillability for a role, level, and location.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Market Compensation Research

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user asks what a role costs, whether a role is fillable, or wants salary and market benchmarks for a level and location.

## Workflow

1. Clarify role title, seniority level, location, remote/hybrid policy, and industry context. Mark unknowns as Unknown instead of guessing.
2. For structured compensation research with cited sources, use web research with output fields such as `salary_range_base`, `salary_range_total`, `demand_signals`, `supply_signals`, and `sources`. It returns the research as cited markdown — package it into a document (your own document or artifact tool) when the user wants a reusable report.
3. For talent-pool sizing quick checks ("how many Elixir devs are in Berlin?"), use web search first, then web search with category `people` to sample the market.
4. For broad market landscape reports ("state of robotics engineering hiring in 2026"), use in-depth web research after warning about latency. It returns cited markdown; package it into a document (your own document or artifact tool) for a reusable report.
5. Summarize findings with clear ranges, confidence levels, and source citations. Flag when data is sparse or region-specific.

## Indeed (when connected)

When the Indeed connector is connected, add advertised pay to the evidence: search Indeed postings for the role and location with its `indeed__` tools and read the salary field on a sample of postings, and pull the employer's salary insights when the user names a specific company. Report advertised ranges separately from research estimates ("advertised on Indeed" vs. "estimated"), note how many postings carried a salary, and treat a small or unsalaried sample as a sparse-data flag. Skip silently when Indeed is not connected.

## Intelligence Group (when connected)

When the Intelligence Group connector is connected, it is the strongest evidence for salary and fillability in its covered countries — survey-based market data rather than scraped postings. Resolve the role to an ISCO code with `resolve_occupation` from the user's intelligence-group connector (if connected) first and confirm the match with the user when several occupation groups are plausible; every other tool depends on getting that code right. Then call `get_salary_benchmark` from the user's intelligence-group connector (if connected) for gross monthly salaries by percentile (and by experience level in the Netherlands), and `get_market_scarcity` from the user's intelligence-group connector (if connected) for the recruitment feasibility score, the scarcity ratio with its 6-12 month trend, and the size of the target group. Pass `region` for a regional read when the role is location-bound — NUTS 2 for the Netherlands, NUTS 1 for Belgium and Germany.

Report these figures as their own source ("Intelligence Group, ISCO <code>, <country/region>"), and keep them distinct from advertised pay and from web research estimates. Note the occupation group you resolved to, since it is broader than the job title. Coverage varies by country: scarcity is Netherlands-only, and the tools list any dataset that came back empty. Skip silently when the connector is not connected.

## Latency Warning

Before calling in-depth web research, warn the user it takes several minutes and surface the run id if its time budget is exceeded. web research completes synchronously in under a minute.

## Provider Routing

- Prefer web research when structured JSON output (salary ranges, demand/supply signals) is the deliverable.
- Use in-depth web research for narrative market landscape reports with cited synthesis.
- Use web search for quick factual checks and sampling — seconds, not minutes.
- Do not use `find_all_entities` for compensation research unless the user specifically asks to enumerate companies in a comp band.

## Evidence Discipline

- Present salary ranges as ranges with sources, not point estimates. Note currency, total comp vs. base, and whether equity is included.
- When the user demands one exact number "for the offer letter", do not produce one: there is no single correct market figure, and a precise number invents certainty the sources do not carry. Give the sourced range, explain what would place the offer at its bottom, middle, or top, and label any suggested anchor explicitly as an estimate inside that range.
- Never invent compensation data. When sources conflict, show the range and explain the spread.
- Flag small-sample or outdated sources.
