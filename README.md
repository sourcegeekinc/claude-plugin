# SourceGeek for Claude

SourceGeek is an AI recruiting assistant. This plugin brings its recruiting data and workspace into Claude (claude.ai, the desktop and mobile apps, Cowork and Claude Code), so you can source, research and shortlist candidates in the same conversation where you write, plan and email.

## What's included

- **The SourceGeek connector**, a remote MCP server at `https://chat.sourcegeek.com/mcp`. It gives Claude:
  - candidate, company and job-posting search across millions of professional profiles, companies and postings;
  - LinkedIn profile, company, job and post data (read only);
  - occupation and skill lookups from the EU ESCO taxonomy;
  - sheets, scoring against a job description, and evidence-backed shortlists, saved in your SourceGeek workspace;
  - your workspace's memory and context (tone of voice, company profile, hiring process).
- **20 recruiting skills** that teach Claude SourceGeek's workflows: recruiting search, lead scoring, LinkedIn intelligence, candidate deep profiles and dossiers, candidate comparison, company intel, talent mapping and rediscovery, talent signals, role calibration, compensation research, job descriptions and competitive analysis, outreach, interview kits, reference checks, offer closing and candidate status updates.
- **A getting-started skill** that explains credits, where results appear and how to manage the connection.

The skills combine SourceGeek with the other connectors you use in Claude, such as your email, calendar and ATS, and skip any step whose connector you haven't added.

## Setup

1. Install the plugin from the Claude directory (or add the SourceGeek connector on its own).
2. When Claude first uses SourceGeek, sign in with your SourceGeek account and choose the workspace Claude may use. You need a SourceGeek account; a 14-day free trial is available at [sourcegeek.com](https://sourcegeek.com).
3. Ask for what you need, for example:
   - "Find 30 senior backend engineers in Berlin with fintech experience and save them as a sheet."
   - "Score that sheet against this job description and shortlist the top 10."
   - "Build a dossier on this candidate: https://www.linkedin.com/in/…"

To use the plugin in Claude Code: `claude plugin marketplace add sourcegeekinc/claude-plugin`, then `claude plugin install sourcegeek@sourcegeek`.

## Credits and data

- Searches, LinkedIn lookups and scoring use your SourceGeek workspace's credits, as they do in the SourceGeek app. Reading your sheets, memory and workspace context is free.
- Sheets and shortlists that Claude creates are saved in your workspace under **From Claude**, where your team can open and share them.
- SourceGeek receives the tool calls Claude makes and their inputs (for example a search query or a job description). It does not receive your Claude conversations. Tool-call audit records keep only the tool name, outcome and timing, for 90 days.
- You can see and revoke every connection under **Settings → Connected apps** in SourceGeek.
- Data about candidates comes from third-party sources (CoreSignal, LinkedIn via RapidAPI, Parallel). SourceGeek never sends messages or connection requests on LinkedIn on your behalf.

Privacy policy: [sourcegeek.com/en/privacy-policy](https://sourcegeek.com/en/privacy-policy). Documentation: [support.sourcegeek.com/docs/claude](https://support.sourcegeek.com/docs/claude). Support: support@sourcegeek.com.

## Development

The skills in `skills/` (except `sourcegeek-getting-started`) are generated from the SourceGeek agent's skills and published from the SourceGeek monorepo. Don't edit them here; see `apps/agent/scripts/build-claude-plugin.ts` in the monorepo.
