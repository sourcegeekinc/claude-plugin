---
name: recruiting-memory
description: Identifies, stores, applies, and manages durable recruiting preferences and constraints — hiring-manager preferences, do-not-source rules, outreach style — with explicit user control. Use when the user says "remember this", asks what is remembered, wants a memory changed or deleted, or states a preference that should persist across chats.
---

<!-- Generated from apps/agent/src/agent/skills by apps/agent/scripts/build-claude-plugin.ts. Do not edit. -->

# Recruiting Memory

> SourceGeek skill for Claude. Candidate, company and job search, LinkedIn data, sheets, scoring, shortlists and workspace memory come from the **SourceGeek connector**. Web research, documents, email, calendar and ATS actions use your own tools and the user's other connectors in Claude; skip any step whose connector isn't available and say so.

Use this skill when a preference surfaces that should outlive the current chat: the user asks to remember or forget something, asks what SourceGeek remembers, or states a durable rule ("never source from customer companies", "this hiring manager rejects management-heavy profiles").

## Where Memory Lives

- User memories (`remember`, `forget`, `list_memories`) are personal to the current authenticated user by default and load into every one of their chats. This is the store for recruiting preferences.
- A workspace-shared bucket also exists: `remember` with `shared: true` saves a rule every member's chats inherit. Use it only when the user explicitly says the rule is for the whole team, and tell them any member can edit or delete it. `forget` and `list_memories` take the same flag; label shared memories as shared whenever you show or apply them.
- Workspace context is a separate store of shared company knowledge (profile, background, tone of voice, open jobs), auto-refreshed daily. It is not loaded automatically: retrieve it with `list_workspace_context` / `get_workspace_context` whenever company facts or tone would help. Correct it with `upsert_workspace_context` (workspace admins only) when the user explicitly asks to fix company information — never store preferences there.
- Memory is never invisible: say when you save, apply, change, or delete one.

## Workflow

1. Spot the durable preference. If the user explicitly said to remember it, proceed; otherwise propose it first — quote exactly what you would save and ask before saving. Never persist something the user did not agree to keep.
2. Check `list_memories` before saving. If an existing memory covers the same ground, update it by saving to the same key (a key overwrites) instead of adding a near-duplicate — and say which one you replaced.
3. Save with a scoped key so future chats apply it narrowly, not broadly:
   - `role.<role-slug>.<topic>` — one role ("role.founding-engineer.company-size")
   - `hm.<name-slug>.<topic>` — one hiring manager's preferences
   - `company.<company-slug>.<topic>` — one company (do-not-source, customer, competitor)
   - `candidate.<name-slug>.<topic>` — one candidate's history or constraints
   - `outreach.<topic>` — outreach tone and style
   - `workspace.<topic>` — rules that apply everywhere ("workspace.do-not-source-customers"); when the user says the rule is for the whole team, save it with `shared: true`
   The value states the preference, where it came from (who said it, when), and any expiry the user gave.
4. Apply remembered preferences only where their scope matches — a rule for one role or one hiring manager does not transfer to another. When one shapes a search, score, plan, or draft, say so ("applying your saved preference X — tell me to drop or update it"). A remembered preference is not a confirmed current requirement: confirm before letting an old memory hard-filter candidates, and flag memories that look stale or past their expiry instead of silently applying them.
5. Correct and delete on request: overwrite via `remember` with the same key, or delete with `forget` (it requires the user's approval). When asked "what do you remember?", show `list_memories` results with each key and value so items are easy to remove. Chat is the only place users manage their personal memories; workspace admins can additionally review shared memories in workspace settings → Context & memory.

## Guardrails

- Never store protected-class preferences — age, gender, family status, pregnancy, nationality, ethnicity, religion, disability, health — or proxies for them (graduation years, "culture fit" meaning demographics), even when the user or a hiring manager states one. Decline, say why, and offer to capture a job-related version only if one genuinely exists.
- Never store personal judgments unrelated to job fit, and never store passwords, tokens, payment data, or one-time codes.
- Do not launder a hiring manager's remark into fact: store it as an attributed preference ("Jan prefers …, said 2026-08-28"), not as a truth about candidates.
