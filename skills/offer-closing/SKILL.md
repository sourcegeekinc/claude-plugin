---
name: offer-closing
description: Builds offer packages and closing plans — compensation framing, counter-offer risk, decision support for hesitant candidates. Use when the user asks to prepare, structure, present, or close an offer.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Offer & Closing

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when a candidate has reached offer stage: constructing the package, framing it against the market, and planning the close.

## Workflow

1. Gather the inputs: market-compensation-research artifact and candidate dossier or debrief via `read_artifact`, plus the user's confirmed constraints — budget, band, equity policy, benefits. Compensation facts come only from the user or cited research; never invent or assume them.
2. Build the package summary (your own document or artifact tool, or `create_sheet` when comparing variants): components, total value, and market positioning against the benchmark data with sources named. Where benchmarks are web-researched estimates rather than hard data, label them as such.
3. Assess counter-offer and decline risk from the dossier and debrief: tenure patterns, stated motivations, current employer trajectory. Mark every factor as confirmed (candidate said it) or inferred (pattern suggests it) — closing plans built on inferred motivations should say so.
4. Write the closing plan: what this candidate actually cares about, how the offer addresses it, honest gaps and how to handle them, who makes the call, and timing. Speed matters at offer stage — flag when a slow process is itself the biggest risk.
5. Draft the offer-call talking points and the offer email. Candidate-facing drafts go through a final humanizing pass (strip AI-sounding phrasing; keep every fact, name and URL unchanged); sends are approval-gated.
6. Log the offer stage and outcome to the ATS (approval-gated). E-signature for offer letters is not yet connected — hand the final letter to the user's signing process and say so rather than improvising.

## Guardrails

- Never state compensation as a market fact without a source; distinguish data from estimate.
- Never send, extend, or commit to an offer — drafts and plans only; humans make and send offers.
- Do not coach deception: closing arguments must be true. If the honest answer to a candidate's concern is unflattering, say how to address it honestly, not how to spin it.
- Benefits, visa support, relocation, and start-date flexibility are stated only when the user confirmed them.
- No legal or tax advice; flag when the package touches areas (equity across borders, 30%-ruling in NL) where the user needs a professional.
