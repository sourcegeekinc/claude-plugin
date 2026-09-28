---
name: reference-checks
description: Prepares reference checks — targeted questions, referee outreach drafts, structured write-ups. Use when the user asks to run references, draft reference questions, or document a reference call.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Reference Checks

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user is checking references for a candidate at or near offer stage.

## Workflow

1. Confirm consent first: the candidate has agreed to references being contacted, and these referees specifically. Backchannel references (people the candidate did not name) are only prepared after the user explicitly confirms they want that, with a note that in the EU this carries GDPR risk and can burn the candidate at their current employer.
2. Read the dossier and debrief (`read_artifact`) and target the questions: confirm the strengths the offer rests on, and probe the open concerns from interviews. Generic reference scripts waste the call.
3. Produce 6–10 questions per referee, matched to what that referee can actually know (a peer cannot speak to budget ownership). Include one verification question (role, dates, working relationship) and close with "would you work with them again" plus space for unprompted comments.
4. Draft the referee outreach via Gmail or Outlook (approval-gated, through a final humanizing pass (strip AI-sounding phrasing; keep every fact, name and URL unchanged)) and offer a booking link from the calendar connectors for the call itself.
5. Provide a write-up template with your own document or artifact tool: referee name and relationship context, verification facts, per-question answers separating verbatim quotes from paraphrase, and flags. After the call, structure the user's raw notes into it.
6. Attach the completed write-up to the candidate in the ATS (approval-gated).

## Guardrails

- No referee contact without confirmed candidate consent; current-employer referees only when the candidate explicitly cleared it.
- Questions must not touch protected characteristics — the same rules as interviews apply to referees.
- Record what the referee said, not what the user hoped to hear; a lukewarm reference is reported as lukewarm.
- Reference content is sensitive personal data: it goes into the ATS record, not into casual Slack messages.
