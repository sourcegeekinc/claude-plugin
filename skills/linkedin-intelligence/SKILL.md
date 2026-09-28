---
name: linkedin-intelligence
description: Routes LinkedIn-native questions — people, companies, jobs, posts, engagement, articles — to the linkedin_* tools and chains their ids correctly.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# LinkedIn Intelligence

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill whenever the answer lives on LinkedIn itself: a member's profile, activity or network; a company page, its jobs or workforce analytics; a job posting and its hiring team; posts, comments, reactions or articles. Every `linkedin_*` tool calls the same real-time LinkedIn data provider (RapidAPI); results are cached, so repeating a call is free within its cache window.

## Id resolution order

1. **People**: accept a username or profile URL everywhere (`usernameOrUrl`). Hashed `/in/ACoAA…` URLs go through `linkedin_profile_by_url`; everything else uses `get_linkedin_contact_details` for the profile itself.
2. **Places**: resolve a city/region/country once with `linkedin_search_locations` and reuse the `geoId` in `linkedin_search_people`, `linkedin_search_jobs` and `linkedin_search_companies`.
3. **Companies**: vanity name or company URL → `linkedin_company_details` (gives `companyId`). Numeric-id URLs → `linkedin_company_details_by_id`. Website domain → `linkedin_company_by_domain`. Employee counts and job counts need the numeric `companyId`.
4. **Posts**: every post tool returns `urn` (activity id) and `shareUrn` (`urn:li:ugcPost:…`). Comments and reposts take the `urn`; comment reactions take the comment's urn plus the post's `shareUrn`.
5. **Jobs**: job id or `jobs/view/<id>` URL → `linkedin_job_details` / `linkedin_job_hiring_team`.

## Workflows

**Candidate activity check** — `linkedin_profile_recent_activity_time` (is the person active?) → `linkedin_profile_posts` and `linkedin_profile_comments` (what they talk about) → `linkedin_profile_about` (account trust) → feed the `outreach-writing` skill.

**Engagement-based sourcing** — `linkedin_search_posts` or `linkedin_search_posts_by_hashtag_v2` (e.g. `#opentowork`, "we are hiring") → `linkedin_post_reactions`, `linkedin_post_comments`, `linkedin_post_reposts` on the best posts → the people returned are leads; enrich the promising ones with `get_linkedin_contact_details` and package with `create_sheet`. When the Expandi connector is connected, offer to push the shortlist into an Expandi lead list (`expandi__*` tools) so the `outreach-writing` skill can draft the campaign.

**Company hiring picture** — `linkedin_company_details` → `linkedin_company_jobs` / `linkedin_company_jobs_count` → `linkedin_job_hiring_team` on a posting to find the recruiter → `linkedin_company_insights` only when headcount growth, hiring trends or alumni matter (it costs five lookups).

**Look-alike expansion** — `linkedin_similar_profiles` from one strong candidate, or `linkedin_company_similar_pages` from one target company.

**Interest signals** — `linkedin_profile_company_interests`, `linkedin_profile_group_interests`, `linkedin_profile_school_interests`, `linkedin_profile_newsletter_interests`, `linkedin_profile_top_voice_interests`, `linkedin_profile_reactions` for personalization; `linkedin_group_posts` to mine a community.

## Cost discipline

- Every call costs credits; cached repeats do not. Do not re-run a search with cosmetic changes.
- `linkedin_company_insights` costs 5×; `linkedin_profile_full_with_posts` and `linkedin_profile_with_recommendations` cost 2× — prefer the specific single-purpose tools unless everything is needed at once.
- `linkedin_search_people` returns at most 10 public profiles per page and is keyword-based; for filterable candidate sourcing use `fast_search_leads` and reserve LinkedIn search for LinkedIn-specific lookups.
- Use `linkedin_search_jobs_v2` only when a distance radius matters; otherwise `linkedin_search_jobs`.
- `linkedin_get_social_post`, `linkedin_search_posts_by_hashtag` (v1) and `linkedin_post_comments_by_share_url` are fallbacks for their primary counterparts.

## Evidence discipline

- Treat every field as LinkedIn self-reported data with the fetch date; engagement counts are snapshots.
- Never infer contact details, willingness to move or protected characteristics from activity data.
- Cite the tool and date when a dossier or shortlist uses these signals.
