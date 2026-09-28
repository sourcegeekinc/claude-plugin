---
name: talent-signals-monitor
description: Surfaces sourcing opportunities from recent layoffs, funding rounds, leadership changes, and other talent-market events.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Talent Signals Monitor

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user asks about recent layoffs, funding events, leadership changes, or other news that opens sourcing windows. It runs on demand; for a standing watch, set it up as a recurring automation (see Recurring Monitoring).

## Workflow

1. Identify the trigger type and scope: industry, geography, company size, role type, and time window (default: last 30 days unless specified).
2. Use web search with category `news` and `startPublishedDate` filters to find recent events.
3. Use web fetch to read the most relevant articles and extract details: affected teams, headcount impact, timing, and which talent pools become reachable.
4. For broad market scans ("what happened in design-heavy companies this month?"), use in-depth web research only after warning the user about multi-minute latency.
5. Synthesize a "sourcing opportunities" report as markdown and package it into a document (your own document or artifact tool): event summary, affected talent pools, urgency, and suggested `fast_search_leads` queries to run next.

## LinkedIn Signals

For LinkedIn-native signals use the `employee_*` tools (see the `employee-intelligence` skill): `employee_search_posts` / `employee_search_posts_by_hashtag_v2` for #opentowork, layoff and hiring posts, `employee_post_reactions` / `employee_post_comments` to turn a viral post into a lead list, `employee_company_jobs_count` and `employee_company_insights` for hiring-intensity and headcount trends.

## Latency Warning

Before calling in-depth web research, warn the user it takes several minutes. Surface the run id if the time budget is exceeded.

## Provider Routing

- Prefer your web search and fetch tools for recent event discovery — fast and index-backed.
- Use in-depth web research for comprehensive market-wide scans that need cited synthesis across many sources.
- Do not use `find_all_entities` or web search for news monitoring — those are for entity list building, not event tracking.

## Recurring Monitoring

When the user wants this watch to run on its own ("alert me when…", "check every week"), set it up with a recurring task (a scheduled task in Claude, or an automation in the SourceGeek app): agree the name, schedule (interval, daily, or weekly, in their timezone), and prompt with the user, and show them the prompt before creating. Each run executes the stored prompt as a fresh agent session with no memory of this chat, so write it self-contained, e.g. "Check for layoffs, funding rounds, and leadership changes at NL fintech companies in the last 7 days and write the sourcing-opportunities report." Match the lookback window to the cadence so events are neither missed nor repeated. Check the user's recurring tasks first to avoid a duplicate watch; pause or resume one with the user's recurring tasks; editing and deleting live on the Automations page.

## Output Guidance

Prioritize actionable signals over news summaries. For each event, state: who becomes reachable, why now, and what search query to run. Flag stale or unconfirmed reports.
