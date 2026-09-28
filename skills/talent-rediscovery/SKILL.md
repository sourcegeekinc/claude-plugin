---
name: talent-rediscovery
description: Finds and explains previously known candidates from ATS, email, Slack, docs, and prior SourceGeek artifacts before sourcing externally.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Talent Rediscovery

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user wants to search existing talent before external sourcing, revisit prior candidates, or re-engage people the company already knows.

## Workflow

1. Confirm the current role target or reuse the active role plan artifact.
2. Offer to search internal history before external sourcing when the user starts a role search.
3. Search connected ATS and communication sources when available:
   - Use Tellent Recruitee MCP tools to search candidates, offers, notes, and stages.
   - Use Gmail, Outlook, Slack, Notion, Google Drive, or OneDrive MCP search when the user mentions those sources.
4. Call `search_rediscovered_talent` with the role description and any connector-sourced candidates as `externalCandidates`. It returns the matched candidates as `csv` (with source, prior status, contact risk, and recommended action as columns); it does not create a sheet. Package them into a sheet artifact with `create_sheet` (pass the returned `csv`) so the user can review them.
5. Explain each rediscovered candidate by name, individually, in the chat reply — a collective summary ("all four came from an earlier search") or pointing at the sheet alone is not enough. Per candidate cover:
   - Where they were found
   - Why they may fit the current role
   - Prior status or outcome
   - Last contact date
   - Known restrictions or contact risk
   - Recommended re-engagement action
   Report unknown fields as unknown; only state a prior status, contact date, or risk that the search actually returned.
6. Use `create_rediscovery_shortlist` when the user wants a shortlist or hiring-manager-ready summary from the rediscovery sheet (pass the artifact id of the sheet you created with `create_sheet`).

## Guardrails

- Search internal sources before recommending external sourcing when the user is starting a new search.
- Preserve prior status and last interaction. Do not reset or hide rejection history.
- Respect `doNotContact` and do-not-contact restrictions. Do not draft outreach for restricted candidates.
- Do not expose sensitive hiring feedback beyond what the user is authorized to see.
- Do not merge candidate identities unless there is strong evidence such as matching email or LinkedIn URL.
- Separate confirmed history from inferred fit. Never invent prior conversations, rejections, or contact dates.

## Connector Guidance

When Recruitee is connected, search ATS records first and pass structured results into `externalCandidates` with:

- `source: "recruitee"`
- `priorStatus` such as rejected, interviewed, applicant, or shortlisted
- `priorOutcome` from notes or stage history when available
- `lastContactDate` when known
- `matchReason` explaining why the prior record may be relevant now

When connector search returns nothing useful, still run `search_rediscovered_talent` to search prior SourceGeek sheets across the workspace.

## Recommended Actions

- Previously rejected: review whether the old concern still applies before re-engaging.
- Previously interviewed or shortlisted: re-engage with a role-specific update.
- Warm conversations: follow up with one concrete reason to reconnect.
- Never contacted: safe to introduce if fit looks strong.
- Do not contact: keep out of outreach flows and say why.
