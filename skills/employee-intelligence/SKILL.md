---
name: employee-intelligence
description: Routes LinkedIn-native questions — people, companies, jobs, posts, engagement, articles — to the employee_* tools and chains their ids correctly.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# LinkedIn Intelligence

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill whenever the answer lives on LinkedIn itself: a member's profile, activity or network; a company page, its jobs or workforce analytics; a job posting and its hiring team; posts, comments, reactions or articles. Every `employee_*` tool calls the same real-time LinkedIn data provider (RapidAPI); results are cached, so repeating a call is free within its cache window.

## Id resolution order

1. **People**: accept a username or profile URL everywhere (`usernameOrUrl`). Hashed `/in/ACoAA…` URLs go through `employee_profile_by_url`; everything else uses `get_employee_contact_details` for the profile itself.
2. **Places**: resolve a city/region/country once with `employee_search_locations` and reuse the `geoId` in `employee_search_people`, `employee_search_jobs` and `employee_search_companies`.
3. **Companies**: vanity name or company URL → `employee_company_details` (gives `companyId`). Numeric-id URLs → `employee_company_details_by_id`. Website domain → `employee_company_by_domain`. Employee counts and job counts need the numeric `companyId`.
4. **Posts**: every post tool returns `urn` (activity id) and `shareUrn` (`urn:li:ugcPost:…`). Comments and reposts take the `urn`; comment reactions take the comment's urn plus the post's `shareUrn`.
5. **Jobs**: job id or `jobs/view/<id>` URL → `employee_job_details` / `employee_job_hiring_team`.

## Workflows

**Candidate activity check** — `employee_profile_recent_activity_time` (is the person active?) → `employee_profile_posts` and `employee_profile_comments` (what they talk about) → `employee_profile_about` (account trust) → feed the `outreach-writing` skill.

**Engagement-based sourcing** — `employee_search_posts` or `employee_search_posts_by_hashtag_v2` (e.g. `#opentowork`, "we are hiring") → `employee_post_reactions`, `employee_post_comments`, `employee_post_reposts` on the best posts → the people returned are leads; enrich the promising ones with `get_employee_contact_details` and package with `create_sheet`. When the Expandi connector is connected, offer to push the shortlist into an Expandi lead list (`expandi__*` tools) so the `outreach-writing` skill can draft the campaign.

**Company hiring picture** — `employee_company_details` → `employee_company_jobs` / `employee_company_jobs_count` → `employee_job_hiring_team` on a posting to find the recruiter → `employee_company_insights` only when headcount growth, hiring trends or alumni matter (it costs five lookups).

**Look-alike expansion** — `employee_similar_profiles` from one strong candidate, or `employee_company_similar_pages` from one target company.

**Interest signals** — `employee_profile_company_interests`, `employee_profile_group_interests`, `employee_profile_school_interests`, `employee_profile_newsletter_interests`, `employee_profile_top_voice_interests`, `employee_profile_reactions` for personalization; `employee_group_posts` to mine a community.

## Cost discipline

- Every call costs credits; cached repeats do not. Do not re-run a search with cosmetic changes.
- `employee_company_insights` costs 5×; `employee_profile_full_with_posts` and `employee_profile_with_recommendations` cost 2× — prefer the specific single-purpose tools unless everything is needed at once.
- `employee_search_people` returns at most 10 public profiles per page and is keyword-based; for filterable candidate sourcing use `fast_search_leads` and reserve LinkedIn search for LinkedIn-specific lookups.
- Use `employee_search_jobs_v2` only when a distance radius matters; otherwise `employee_search_jobs`.
- `employee_get_social_post`, `employee_search_posts_by_hashtag` (v1) and `employee_post_comments_by_share_url` are fallbacks for their primary counterparts.

## Evidence discipline

- Treat every field as LinkedIn self-reported data with the fetch date; engagement counts are snapshots.
- Never infer contact details, willingness to move or protected characteristics from activity data.
- Cite the tool and date when a dossier or shortlist uses these signals.
