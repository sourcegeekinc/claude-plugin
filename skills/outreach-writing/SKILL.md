---
name: outreach-writing
description: Drafts recruiting outreach. Use when the user asks for email, LinkedIn messages, follow-ups, sequences, personalization, reply drafting, tone changes, or candidate outreach.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Outreach Writing

> SourceGeek skill for Claude. Candidate, company and job search, employee data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when the user wants concise recruiting messages for candidates not yet in process: first contact, cold follow-ups, and reply handling. For candidates already in the pipeline — rejections, keep-warm notes, interview prep, offer-stage updates — use candidate-status-comms.

## Workflow

1. Identify the recipient, goal, channel, tone, and call to action. Fetch the workspace's tone of voice first (`get_workspace_context` with key `tone-of-voice`) and default to it when it exists; only deviate when the user asks for a different tone.
2. Use verified details from tool output or artifacts for personalization.
3. Keep the first message short and specific. Avoid generic praise and unsupported claims.
   Never leave placeholder tokens in a draft ("[Your name]", "{{company}}", "<role>"): a message that "can be sent with minimal editing" contains none. Sign off with the sender's actual name when it is known; otherwise end the message before the signature line rather than emitting a placeholder.
4. For follow-ups, add one new useful reason to respond rather than repeating the first message.
5. Offer variants only when useful: shorter, warmer, more direct, or more senior.
6. Before showing any drafted message to the user, run it through a final humanizing pass (strip AI-sounding phrasing; keep every fact, name and URL unchanged) and present the returned text exactly. Do this for each variant. It strips AI-sounding tells while preserving facts and URLs, and is a fast no-op when the draft already reads human.

## WhatsApp

When the channel is WhatsApp (requires the WhatsApp Business connector):

1. WhatsApp is never a cold channel: the candidate must have opted in to WhatsApp contact, and the first touch to a new candidate goes over email or LinkedIn, not WhatsApp.
2. Outside the 24-hour session window (no message from the candidate in the last 24 hours), only pre-approved templates can be sent. Call `list_message_templates` from the user's whatsapp-business connector (if connected) first and pick an APPROVED template; draft the personalization as the template's parameter values, not as free text. Do not promise wording the template cannot express.
3. Inside the 24-hour window (the candidate replied recently), free-form replies via `send_text_message` from the user's whatsapp-business connector (if connected) are allowed; keep them short and run them through a final humanizing pass (strip AI-sounding phrasing; keep every fact, name and URL unchanged) like any other draft.
4. Every send is approval-gated — show the user exactly what will go out before they approve.

## Expandi (LinkedIn campaigns)

When the user wants LinkedIn outreach sent at scale and the Expandi connector is connected (`expandi__*` tools):

1. Draft the message copy in chat first (steps 1–6 above), then hand the approved copy to Expandi: build or extend a lead list, then draft the campaign steps with that copy. Personalize with verified details only; Expandi placeholders such as first name are fine, invented facts are not.
2. Expandi never launches a campaign or sends a reply on its own — every campaign and draft it prepares waits for the user to review and launch inside Expandi, and it stays within each LinkedIn account's warm-up and daily limits. Say so instead of promising a send time.
3. For inbox replies, read the conversation history through the connector before drafting, keep the reply short, and run it through a final humanizing pass (strip AI-sounding phrasing; keep every fact, name and URL unchanged) like any other draft.
4. Do not create duplicate leads or campaigns: check existing lead lists and campaigns before adding, and reuse the one the user names.

## Style Guidance

Write like a practical operator. Prefer plain language, one clear ask, and a message that can be sent with minimal editing.
