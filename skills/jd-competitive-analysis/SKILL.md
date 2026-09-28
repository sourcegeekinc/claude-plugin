---
name: jd-competitive-analysis
description: Compares job descriptions against competitor postings to extract winning patterns and suggest concrete JD improvements.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# JD Competitive Analysis

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user wants to compare their job description against competitor postings, extract hiring patterns, or rewrite a JD using market-proven language.

## Workflow

1. Identify the role, level, location, and named competitors (or ask for them if missing).
2. Use web search to find live job postings for the same role at named competitors.
3. Use web fetch to read the postings. Extract patterns: required skills, comp transparency, benefits highlighted, selling points, and red flags.
4. If the user pasted their JD, compare it side-by-side against competitor patterns. If not, offer to pull it from a URL via web fetch.
5. Build the comparison as markdown and package it into a document (your own document or artifact tool): competitor patterns table, gaps in the user's JD, strengths to keep, and specific rewrite suggestions.
6. Use your own review of the document on the artifact when the user wants inline edit suggestions for their JD.
7. Pair with the `role-calibration` skill when the user also needs structured search criteria or a scoring rubric derived from the competitive analysis.

## Indeed (when connected)

When the Indeed connector is connected, use its `indeed__` tools as the primary source of live competitor postings before falling back to web search: search postings for the role and location, then open the full posting for each named competitor to extract requirements, comp transparency, and selling points. Indeed postings carry the advertised salary and the employer's posting date, so cite both in the comparison table and link each pattern to the posting it came from. Skip silently when Indeed is not connected and continue with web search.

## Provider Routing

- Prefer your web search and fetch tools for finding and reading job postings — fast, with highlights.
- Use web fetch when the user provides a specific JD URL; it returns the page as markdown, which you can package with your own document or artifact tool if the user wants to keep it.
- Do not use in-depth web research, web research, or `find_all_entities` for JD comparison — they are overkill for reading live postings.

## Output Guidance

Recommend concrete edits, not generic advice. Quote competitor patterns where useful. Flag when competitors are unusually transparent about comp or unusually vague about requirements.
