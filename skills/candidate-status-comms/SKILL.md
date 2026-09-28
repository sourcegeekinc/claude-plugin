---
name: candidate-status-comms
description: Drafts communications to candidates already in process — rejections, keep-warm notes, interview prep emails, offer-stage updates. Use when the user asks to reject, update, nudge, or prep a candidate in the pipeline.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Candidate Status Comms

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill for messages to candidates who are already in the process. For first-contact sourcing messages and cold follow-ups, use outreach-writing instead.

## Workflow

1. Establish the candidate's actual stage before drafting — from the ATS or a SourceGeek artifact, not from assumption. The wrong template at the wrong stage damages trust more than silence.
2. Pick the message type and follow its rules:
   - **Rejection**: prompt, warm, and short. Thank them for the specific time invested, state the decision clearly in the first two sentences, optionally one honest non-protected reason (e.g. "we went with someone with deeper payments experience"), and keep the door open only when the interest is genuine. Never fabricate a reason and never leave the decision ambiguous.
   - **Keep-warm**: one concrete update or reason to stay engaged. If there is nothing new to say, say when there will be — never send filler.
   - **Interview prep**: logistics restated (time with timezone, format, link), panel names and roles, what the stage focuses on, and any prep the team genuinely expects.
   - **Offer-stage update**: factual and prompt; timing promises only when confirmed by the user.
3. Run every candidate-facing draft through a final humanizing pass (strip AI-sounding phrasing; keep every fact, name and URL unchanged) and present the returned text exactly.
4. Send via Gmail or Outlook after approval, or hand the draft to the user for their own channel. WhatsApp may be used only if a WhatsApp connector is available and the candidate has already opted in to that channel — never for a first touch.
5. Log every sent communication to the ATS (approval-gated) so the candidate record reflects what they were told and when.

## Guardrails

- Rejection reasons must never reference or hint at protected characteristics, and avoid proxy phrasing ("overqualified", "not a culture fit" without substance).
- Never promise feedback, timelines, or future roles the user has not confirmed.
- Rejected candidates get told — if the user wants to skip rejections, note the candidate-experience cost once, then follow their decision.
- One message per event; do not stack a rejection and a talent-pool pitch into the same send unless the interest is real.
