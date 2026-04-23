# Four-Pass EOD Pattern

A reusable template for end-of-day sync between a personal AI operating layer and a shared team knowledge base.

## What this is

If you run your own work through a private AI agent stack (personal chats, local project files, working scratchpad), and your team runs a separate shared knowledge base (a Git repo of Markdown, a CRM file, a Slack channel), you face a recurring problem: what did I ship today that the team needs to see, in what format, pushed where?

The Four-Pass EOD Pattern is the batching protocol I use at the end of each substantive day. It runs four sequential passes in a single session of my personal agent, produces one drop file, and hands off to a separate team-facing agent session for execution. The split keeps my personal working context out of the shared repo, and keeps the shared repo's permissions and voice conventions out of my personal scratchpad.

## When this pattern applies

Use it when all of the following are true:

1. You have a **personal** AI operating layer with context the team should not see (client-specific workflow detail, draft copy, half-formed plans, internal role structure, effort-level audits).
2. You have a **shared** team knowledge base that needs to stay current with a rolling audit trail (daily updates, CRM changes, program state, pipeline movement).
3. The two systems have **different permissions, voices, and audiences**.
4. End-of-day is the right granularity. Real-time sync is too noisy; weekly sync loses canonical-state changes.

If you only have one system, you do not need this pattern. If the shared system IS your working scratchpad, you do not need this pattern. This is a bridge protocol for two-system setups.

## The four passes

Each pass runs in order. Pass N's output feeds Pass N+1. All four write into a single gitignored drop file. A separate team-facing agent picks up the drop file and executes.

1. **Scan canonical-state changes.** Review today's activity in your personal layer. Flag anything that should update the shared knowledge base (new contacts, pipeline movement, program state, decisions, policy changes, status markers).
2. **Draft the commit manifest.** For each flagged change, write the repo path, change description, proposed new content, and push style (direct-to-main vs PR). The personal agent does not edit the shared repo; it drafts the edits.
3. **Stage the daily update file.** Write the day's rolling-audit-trail entry at the shared repo's expected path. Project-sectioned, info-dense, in your own voice (no internal-agent-structure leakage).
4. **Draft the team broadcast.** Write the tight Slack or team-channel summary. Ownership tags in trailing parens. Short enough to skim, rich enough to orient.

The drop file is the handoff. One artifact, four sections, ready for the team-facing agent to execute.

See [`templates/`](templates/) for per-pass detail (purpose, inputs, outputs, prompt skeleton, decision rules, anti-patterns). See [`example/`](example/) for a fully fictional worked example demonstrating the pattern end to end.

## Why four passes, in this order

Each pass has a different job and a different failure mode. Bundling them fails quietly.

- **Pass 1 (scan) before Pass 2 (manifest)** because the manifest only knows what to commit once you have inventoried what changed.
- **Pass 2 (manifest) before Pass 3 (daily update)** because the manifest is the reference list of substantive items; the daily update is the narrative version of that list. Writing the narrative first produces gaps.
- **Pass 3 (daily update) before Pass 4 (broadcast)** because the broadcast is a TL;DR of the daily update. Writing the broadcast first produces a tight summary of the wrong things.

The order is not decorative. Each pass depends on the previous one.

## What goes in, what stays out

The hard rule for any two-system setup is a clear wall between what crosses and what does not.

**Crosses into the shared repo and team broadcast:**
- Substantive ships across projects
- Canonical-state changes (contacts, pipeline, program details, roles)
- Team meetings, upcoming events, cross-team decisions
- Course or curriculum changes
- Financial actuals, invoices, projections
- Waiting-on items (external and internal)
- External activity: press, conferences, partnerships

**Stays in the personal layer:**
- Email and message draft **content** (meta "email sent to X" is fine; quoting the draft is not)
- Pre-send marketing copy drafts
- Client-specific workflow notes with confidential detail
- Internal agent names, team structure, role-play scaffolding in the personal layer
- Effort-level audits and internal QA
- Your own working scratchpad and half-formed plans

The list above reflects my setup; yours will differ. The important move is to write your own list down and check against it every pass. Without a written wall, drift is the default.

## Voice rules

The daily update and broadcast go to a team audience that does not share your personal agent's internal vocabulary. First person from your own perspective. Never reference internal agent names, role-play scaffolding, or personal-layer tooling by its internal handle. Describe functionally. If your personal layer has a proper name, do not use that name in any artifact that crosses into the shared repo.

## Auth model

My setup splits authentication across two environments:

- **Personal agent session (sandboxed).** Read-only on the shared repo for state checks. Writes only to the gitignored drop zone. Never calls the team chat API directly.
- **Team-facing agent session (separate).** Reads the drop file, commits locally, calls the team chat API to draft (never auto-send) a broadcast.
- **Network git (push, pull, fetch).** Runs in a terminal where the GitHub CLI is authenticated. The sandboxed agents never hold repo credentials.

The split means the sandbox never gets credentials it should not have, and the team chat never gets content from the personal layer without a human in the loop.

## How to adapt this to your stack

The pattern is opinionated on the four passes and the drop-file contract. Everything else is fungible.

- **Shared repo.** Any Git repo with a Markdown-first convention works. I use one private GitHub repo with a folder for daily updates and a CRM-style file for contacts.
- **Personal layer.** Any AI agent stack that can read local files and your inbox, calendar, and Slack works. I use a Cowork session with file access, Gmail MCP, Google Calendar MCP, and a read-only mount of the shared repo.
- **Team broadcast.** Any team channel works. I use Slack with a private channel for the team, using the draft-first flow (draft via MCP, review inline, send from the UI).
- **Trigger.** One command phrase to the personal agent. Mine is `end of day`. The agent runs all four passes and writes the drop file.

## Repo layout

```
four-pass-eod-pattern/
  README.md                         you are here
  BLOG.md                           builder-voice design rationale
  LICENSE                           MIT
  templates/
    01-scan-canonical-state.md      Pass 1: inventory of changes
    02-commit-manifest.md           Pass 2: proposed repo edits
    03-daily-update.md              Pass 3: rolling audit trail entry
    04-team-broadcast.md            Pass 4: tight team-channel summary
  example/
    sample-eod-drop.md              one full fictional day
```

## Credits and context

This pattern is an extracted, standalone recipe from cLawEO, [The Atticus Project](https://www.atticusprojectai.org)'s CEO-level agentic operating system. cLawEO implements the Agentic Knowledge OS framework from the Axur practitioner's report: Jônadas Techio and Fábio Ramos, "The Agentic Knowledge Operating System: A Practitioner's Report," v0.3, April 2026. The end-of-day protocol emerged after about a dozen daily runs during April 2026. The stable order is the point: the first runs were reordering and renaming, and once the four passes settled, the drift stopped.

If you run a similar two-system setup and adapt this pattern, I am curious how your passes shift. Issues and pull requests welcome.

## License

MIT. See [`LICENSE`](LICENSE).
