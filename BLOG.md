# AI chief of staff meets AI CEO: a workable sync framework

Atticus has gone fully agentic. A team of two is operating at the level of two full-stack teams with a dozen employees: we made the dream work.

The engine is a simple end-of-day sync protocol between two AI agents, and this post shares it.

There was considerable friction going into this: I run two AI systems in my work, and they do not always communicate. One is my personal chief of staff agent. It tracks my todos, drafts my emails, holds the half-formed plans and the client-specific notes that should never cross into a shared view. The other is cLawEO, [The Atticus Project](https://www.atticusprojectai.org)'s CEO-level agentic operating system, named for the c[law]EO play on CLO/CEO/law. It lives in a Git repo of Markdown the whole team reasons over: contacts, program state, financials, a rolling daily-update audit trail, a team Slack channel built on top. cLawEO is our implementation of the Agentic Knowledge Operating System framework that Jônadas Techio and Fábio Ramos described in their April 2026 Axur practitioner's report.

For about a dozen daily runs, I felt the drift between the two systems. My personal layer always knew more than the shared repo. The Axur paper calls this pattern "silent drift" and names it the default failure mode for two-system setups. It recommends a daily agent-assisted audit. This is the protocol I run.

```mermaid
%%{init: {'theme':'base', 'themeVariables': {
  'primaryColor':'#D5E1E8','primaryBorderColor':'#1B365D','primaryTextColor':'#1B365D',
  'lineColor':'#1B365D','clusterBkg':'#F2F5F7','clusterBorder':'#9CB9C7'
}}}%%
flowchart LR
    subgraph P["Personal layer (sandboxed)"]
        CoS["AI chief of staff<br/>todos, drafts, client notes,<br/>half-formed plans"]
        Drop[["drop file<br/>(gitignored)"]]
        CoS --> Drop
    end
    subgraph S["Shared layer: cLawEO"]
        Repo[("Git repo of Markdown<br/>CRM, programs, financials,<br/>daily-update trail")]
        Slack["Team Slack"]
    end
    Drop -. daily handoff .-> Team["AI CEO<br/>team-facing agent"]
    Team --> Repo
    Team --> Slack
    style Drop fill:#E3F0F3,stroke:#2A7F91,color:#1A6B7D
    style Team fill:#E3F0F3,stroke:#2A7F91,color:#1A6B7D
```

## What I was trying to solve

At end of day I need to answer four questions:

1. What changed today that the shared repo should know about?
2. For each change, which file updates and how?
3. What is today's entry in the rolling audit trail?
4. What is the one-paragraph Slack summary?

I tried answering all four at once and kept failing. Each output needs the previous one as input.

[diagram/graphic]

## The pattern

Four passes. One session of my personal agent. One drop file with four sections. Each pass writes its section and feeds the next.

**Pass 1: Scan.** Inventory today's personal-layer activity. Flag anything that should update the shared repo: new contacts, pipeline movement, program state, decisions, financial actuals, waiting-on items. Inventory only. No drafting.

**Pass 2: Commit manifest.** For each flagged change, draft the precise repo edit: file path, description, proposed content, push style (direct vs PR). My personal agent drafts; a separate team-facing agent executes later.

**Pass 3: Daily update file.** Write today's rolling-audit-trail entry at the repo's expected path. Project-sectioned, info-dense, narrative form of Pass 2 with named specifics throughout.

**Pass 4: Team broadcast.** Tight Slack post. TL;DR of Pass 3. Ownership tags in trailing parens. Draft only, never auto-send; the team-facing agent stages it and I send from the Slack UI.

All four write into one drop file at `team/personal_kat/_pending_commits/YYYY-MM-DD.md`, gitignored, personal-layer only. The drop file is the handoff contract.

```mermaid
%%{init: {'theme':'base', 'themeVariables': {
  'primaryColor':'#D5E1E8','primaryBorderColor':'#1B365D','primaryTextColor':'#1B365D',
  'lineColor':'#1B365D','clusterBkg':'#F2F5F7','clusterBorder':'#9CB9C7'
}}}%%
flowchart TB
    Trigger(["End-of-day trigger"]) --> P1["Pass 1: Scan<br/>inventory canonical-state changes"]
    P1 --> P2["Pass 2: Commit manifest<br/>draft precise repo edits"]
    P2 --> P3["Pass 3: Daily update file<br/>narrative audit-trail entry"]
    P3 --> P4["Pass 4: Team broadcast<br/>TL;DR Slack post"]
    P4 --> Drop[["One drop file,<br/>four sections"]]
    Drop -. team-facing agent picks up .-> Execute(["Commits land, Slack draft stages"])
    style Trigger fill:#F2F5F7,stroke:#9CB9C7,color:#1B365D
    style Drop fill:#E3F0F3,stroke:#2A7F91,color:#1A6B7D
    style Execute fill:#E3F0F3,stroke:#2A7F91,color:#1A6B7D
```

## Why ordering is the point

Each output depends on the previous one. Drafting commits mid-inventory produces commits that miss items surfacing later. Writing the daily update before the manifest produces gaps, because the manifest is the checklist the narrative writes against. Writing the broadcast before the daily update produces a confident summary of an incomplete picture. The stable order is what buys the reliability.

[diagram/graphic]

## What crosses, and what stays

The hardest design question in a two-system setup is the wall.

**Crosses into the shared repo and broadcast:** substantive ships, canonical-state changes (contacts, pipeline, program details, team roles), cross-team decisions, financial actuals, waiting-on items, external activity.

**Stays in the personal layer:** email and message draft **content** ("sent to X" crosses; the draft text does not), pre-send marketing drafts, client-specific workflow detail, internal role-play scaffolding, effort-level audits, personal scratchpad.

Without a written wall, drift is the default. With one, each pass has a test to apply.

[diagram/graphic]

## Auth, and why the split matters

Authentication splits across three environments. The personal agent has read-only access to the shared repo and writes only into the gitignored drop zone. The team-facing agent reads the drop file, commits locally, and drafts the Slack post (never sends). Network git runs in a separate terminal with the GitHub CLI authenticated as me. The sandbox never holds credentials it should not, and the team chat never receives personal-layer content without a human in the loop.

```mermaid
%%{init: {'theme':'base', 'themeVariables': {
  'primaryColor':'#D5E1E8','primaryBorderColor':'#1B365D','primaryTextColor':'#1B365D',
  'lineColor':'#1B365D','clusterBkg':'#F2F5F7','clusterBorder':'#9CB9C7'
}}}%%
flowchart LR
    subgraph E1["Personal agent (sandbox)"]
        A["read-only to shared repo<br/>writes to drop zone only"]
    end
    subgraph E2["Team-facing agent (sandbox)"]
        B["reads drop file<br/>local commits<br/>drafts Slack, never sends"]
    end
    subgraph E3["Mac Terminal (gh authed)"]
        C["push, pull, fetch"]
    end
    E1 -. drop file .-> E2
    E2 -. push command .-> E3
    E3 --> GH[(GitHub)]
    style GH fill:#E3F0F3,stroke:#2A7F91,color:#1A6B7D
```

## Try it

The repo at [katf167/four-pass-eod-pattern](https://github.com/katf167/four-pass-eod-pattern) has the full template set: per-pass prompt skeletons, decision rules, anti-patterns, and a fictional worked example. Opinionated on the four passes and the drop-file contract; everything else is up to you.

The pattern generalizes beyond my setup. If you have a personal AI layer and a shared team knowledge base, you have the two-system problem. Four passes, in this order, has been the stable recipe.

At [Atticus](https://www.atticusprojectai.org) we teach legal teams how to build their own version of this in our [1:1 coaching service](mailto:aicoach@atticusprojectai.org) and in our [AI Habits for Legal Professionals](https://maven.com/the-atticus-project/ai-habits-for-legal-professionals/) cohort on Maven.

[diagram/graphic]
