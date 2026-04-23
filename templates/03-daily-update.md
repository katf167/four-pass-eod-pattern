# Pass 3: Daily Update File

**Runs in:** personal agent session
**Writes to:** Section 3 of the drop file (staged content for `daily/updates/YYYY-MM-DD-{you}.md`)
**Blocks on:** Pass 1 and Pass 2 output
**Feeds:** Pass 4 (team broadcast)

## Purpose

Write the day's entry in the shared repo's rolling audit trail. This file is the persistent record of what shipped, what is coming up, what is blocked. The team's shared agent reasons over this file plus the others in `daily/updates/` to answer questions like "how is Program X looking" or "what has Team Member Y been working on."

The daily update is the narrative version of the commit manifest. Same content, different form. Where the manifest is structured and machine-executable, the update is info-dense and human-readable.

## Inputs

1. Pass 1's flagged changes.
2. Pass 2's commit manifest.
3. The shared repo's voice conventions and project sectioning.
4. Any time-sensitive items flagged mid-day.

## Outputs

A full Markdown file, ready to copy-paste into the shared repo at `daily/updates/YYYY-MM-DD-{you}.md`. Typical structure:

```
# {YYYY-MM-DD} daily update, {you}

## {Project A}
{2 to 5 sentences: what shipped, what changed, what is next}

## {Project B}
{same}

## {Project C}
{same}

## Coming up
{Events, deadlines, decisions expected in the next 1 to 7 days}

## Waiting on
{External and internal blockers, with names and since-when}

## Notes
{Optional: anything flagged mid-day, open questions, context the team should carry forward}
```

## Prompt skeleton

```
Run Pass 3 of the Four-Pass EOD Pattern.

Using Section 1 (canonical-state changes) and Section 2 (commit manifest) of the drop file at {drop file path}, write today's daily update file.

Voice:
  - First person from your own perspective.
  - The entry should be info-dense and project-sectioned. Do not use bullet-point fragments as standalone sentences.
  - No internal-agent names from the personal layer.
  - No personal-layer tooling names; describe functionally.
  - Follow the shared repo's brand voice rules: {paste brand voice rules}

Structure:
  ## {Project name}
  2 to 5 sentences per project. What shipped, what changed, what's next. Named specifics (contacts, counts, dates) beat generics.

  ## Coming up
  Events, deadlines, decisions expected in the next 1 to 7 days.

  ## Waiting on
  External and internal blockers. Name names, include since-when.

  ## Notes (optional)
  Time-sensitive flags, open questions about canonical state.

Write the full file into the drop file under "## 3. Daily update file (staged)" with the path header `Path: daily/updates/{YYYY-MM-DD}-{you}.md`.
```

## Decision rules

- **Project-sectioned beats chronological.** The team's agent reasons over the file by project; a chronological narrative scatters the signal.
- **One section per active project.** If a project had no movement today, leave it out. Silence is allowed.
- **Info-density over brevity.** The team preference for this audit trail is long-form, not TL;DR. Keep the narrative complete; the Slack broadcast in Pass 4 is the TL;DR.
- **Named specifics in every section.** "We shipped the Week 1 reinforcement email to the cohort" beats "shipped cohort materials." The audit trail is worthless without the specifics.
- **Waiting-on items include the since-when.** "Waiting on Partner X since last Thursday" beats "waiting on Partner X." The team needs to know when a blocker is going stale.
- **If a project had mixed news, write both halves.** Do not sanitize. The audit trail is more useful with the real picture than with the optimistic one.

## Anti-patterns

- **Bullet lists masquerading as prose.** This file should read as prose. If you find yourself writing three-word bullets, rewrite them as sentences.
- **Internal-agent-name leakage.** The shared repo does not know your personal layer's internal names. If you find yourself writing "my Chief of Staff agent said," rewrite to describe the action functionally.
- **Commit-manifest restating.** The daily update is the narrative, not the manifest. If you are copy-pasting commit descriptions verbatim, you are skipping the narrative pass.
- **Terse days.** The team reads the long form. Do not shortcut. Senior stakeholders in many real setups explicitly ask for long-form; when they have, respect the preference over generic brevity advice.
- **Project labels that match your personal layer's labels instead of the shared repo's.** If the shared repo calls a project by one name and you call it something else, use the shared repo's name. Consistency across the rolling trail matters more than your internal naming.

## Voice and filter guardrails

Before finalizing, run through this checklist:

1. First person from your own perspective.
2. No internal-agent names from the personal layer.
3. No personal-layer tool names; functional descriptions only.
4. No em or en dashes. Use "to" or reword.
5. No invented-foil binaries ("not just X / it's Y"). State what something IS.
6. No hype language. Say what it is.
7. Named specifics present (counts, dates, names, outcomes).
8. Claims traceable to source activity.

## Handoff

When Pass 3 completes, the drop file has Section 3 populated with a full staged daily-update file. Pass 4 reads this section and writes the broadcast.
