---
name: sourcegeek-getting-started
description: Explains how SourceGeek works in Claude. Use when the user asks what SourceGeek can do, how credits work, where SourceGeek results are saved, how to switch SourceGeek workspace, or how to connect, reconnect or disconnect SourceGeek.
---

# SourceGeek in Claude

SourceGeek is a recruiting data and workspace service, reached through the **SourceGeek connector**.

## What it can do

- Search candidates, companies and job postings (`fast_search_leads`, `fast_search_companies`, `fast_search_jobs`; the `precision_*` variants are slower, cost more and match better).
- Read employee data: public LinkedIn profiles, companies, jobs and posts (`get_employee_contact_details` and the `employee_*` tools). These are read only: SourceGeek never posts, messages or sends connection requests on LinkedIn.
- Save results as sheets (`create_sheet`), score a sheet against a job description (`score_leads`) and turn a scored sheet into a shortlist (`create_shortlist`).
- Find the user's earlier sheets and shortlists (`list_artifacts`, `read_artifact`) and hiring-manager feedback on shortlists (`read_shortlist_feedback`).
- Remember facts and preferences (`remember`, `list_memories`) and read the workspace's context: tone of voice, company profile, hiring process (`list_workspace_context`, `get_workspace_context`).

## Credits

Searches, employee data lookups, enrichment and scoring spend the SourceGeek workspace's credits; reading sheets, memory and context is free. Don't repeat identical searches. If a tool reports the workspace is out of credits, pass on the top-up link it returns and stop spending calls.

## Where results go

Everything SourceGeek creates is saved in the user's SourceGeek workspace, in a thread called **From Claude**, where their team can open, share and keep working on it. Give the user the URL a tool returns when you create a sheet or shortlist.

## Managing the connection

- The connection is tied to one SourceGeek workspace, chosen when the user signed in. To use another workspace, the user disconnects SourceGeek in Claude and connects again, choosing the other workspace.
- The user can see and revoke connections in SourceGeek under **Settings → Connected apps**.
- If a tool says the connection wasn't granted a permission, the user needs to reconnect and allow it.

Help: https://support.sourcegeek.com/docs/claude
