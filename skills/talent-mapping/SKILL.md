---
name: talent-mapping
description: Builds target company lists for sourcing — which companies to poach from based on role, domain, stage, and geography.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Talent Mapping

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user asks which companies to source from, wants a target-company map, or needs to find companies similar to a reference employer.

## Workflow

1. Turn the role or intake into a company thesis: stage, domain, geography, tech signals, and any known reference companies.
2. Start fast: use web search for similar pages on reference company URLs and web search (companies) for initial candidate lists — these return in seconds.
3. Use web search with category `company` when semantic search helps (e.g. "Series B fintech in Amsterdam").
4. When the user has hard match criteria ("Series B+, EU-based, has an ML team, 100+ engineers"), escalate to `find_all_entities` with explicit match conditions — but warn first that it takes several minutes.
5. Build the result as a markdown target-company map and package it into a document (your own document or artifact tool). Include: company name, why it matches, talent pools to mine, and risks (e.g. recently raised → unlikely leavers, recent layoffs → reachable talent). (A table of target companies can instead be packaged as a sheet with `create_sheet`.)
6. Suggest concrete `fast_search_leads` queries the user can run against the strongest target companies.

## Latency Warning

Before calling `find_all_entities`, warn the user it takes several minutes. If the time budget is exceeded, surface the findall id and partial status.

## Provider Routing

- Prefer your web search and fetch tools for fast company discovery and similarity lookups.
- Use Parallel web search for verified entity list building with objective-based matching.
- Use `find_all_entities` only when the user needs verified match-condition lists that simpler search cannot satisfy.
- Do not use in-depth web research or web research for company list building — use them only if the user separately asks for a deep dive on one company.

## Dutch Target Lists (KVK Handelsregister)

When the KVK connector is connected and the map targets the Netherlands, verify and enrich the list against the official business register:

- Confirm each Dutch target with `search_companies` from the user's kvk connector (if connected) and attach its KVK number to the map, so downstream artifacts stay tied to a verifiable registration.
- Use `get_company_profile` from the user's kvk connector (if connected) / `get_establishment_profile` from the user's kvk connector (if connected) for authoritative size signals (employee counts, branch locations) instead of scraped estimates, and SBI activity codes to check the company actually operates in the target domain.
- `get_trade_names` from the user's kvk connector (if connected) resolves companies that operate under a different name than their registration — useful before dismissing a near-match.

If the KVK connector is not connected, build the map from web sources as usual and note that Dutch entries are unverified against the registry.

## Market Sizing (Intelligence Group, when connected)

When the Intelligence Group connector is connected, size the market before mapping companies into it:

- Resolve the profile with `resolve_occupation` from the user's intelligence-group connector (if connected), then `get_market_scarcity` from the user's intelligence-group connector (if connected) for the target group size per region — it tells you whether the map should concentrate on one region or has to span several to reach a workable pool.
- `get_sourcing_channels` from the user's intelligence-group connector (if connected) ranks the job boards, niche boards, and social platforms this group actually uses, which belongs in the sourcing-potential ranking alongside company signals.
- Report the sizing as market context for the map, not as a count of reachable candidates: it covers the occupation group, which is broader than the profile being mapped.

If the connector is not connected, size the market from search-side signals as usual and label it an estimate.

## Output Guidance

Rank companies by sourcing potential, not just similarity. Flag companies where talent is likely reachable (layoffs, stagnation, post-acquisition) vs. unlikely (recent mega-round, hypergrowth hiring).
