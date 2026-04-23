# Pass 4: Team Broadcast

**Runs in:** personal agent session (drafts), team-facing agent session (sends as draft)
**Writes to:** Section 4 of the drop file
**Blocks on:** Pass 3 output
**Feeds:** the team channel, via human review

## Purpose

Write the tight, skimmable team-channel summary of today's movement. This is the broadcast the team reads at the end of the day (or the start of the next morning). It is a TL;DR of the daily update file, organized by what the team cares about: what shipped, what is coming, context they might need, what is blocked.

The broadcast is a draft. It goes to the team channel as a draft, is reviewed by the human, and is sent manually from the team chat UI. No auto-send. This is non-negotiable for a pattern that crosses the personal-team wall.

## Inputs

1. Pass 3's staged daily update file.
2. The team channel's tone conventions.
3. The team channel's ownership-tagging conventions (names, abbreviations, emoji).
4. Any recent context the team asked you to carry forward (a week-old decision, a pending sign-off, an upcoming event).

## Outputs

A ready-to-paste team-channel post, structured as:

```
*Shipped today*
- {item} ({owner})
- {item} ({owner})

*Coming up*
- {item}, {date/time} ({owner})
- {item}, {date/time} ({owner})

*Recent context* (optional, only if load-bearing)
- {context, 1 line}

*Waiting on*
- {person/team}: {what}, since {when}
```

Ownership tags go in trailing parens. Short names, not full names, unless disambiguation is needed. Emoji sparingly if at all; match channel convention.

## Prompt skeleton

```
Run Pass 4 of the Four-Pass EOD Pattern.

Using Section 3 (daily update file) of the drop file at {drop file path}, write the team broadcast for {channel name}.

Structure:
  *Shipped today*
  - (3 to 8 items, most substantive first)

  *Coming up*
  - (Events, deadlines, decisions expected in the next 1 to 7 days)

  *Recent context* (optional)
  - (Only if an older item is load-bearing for today or tomorrow)

  *Waiting on*
  - (External and internal blockers with since-when)

Voice rules:
  - Your voice, if the channel posts under your name.
  - No personal-layer agent names.
  - No hype. State what shipped.
  - No em or en dashes.
  - No invented-foil binaries.

Ownership tags:
  - Trailing parens on each item, short names preferred.
  - Use {channel conventions}.

Length:
  - Skimmable. Most readers check on phone. Keep each bullet to one line where possible.
  - Exception: if your team has asked for long-form, keep the long form. Trust the asked-for preference over generic brevity advice.

Write into the drop file under "## 4. Team broadcast draft."

The team-facing agent will later call the draft-message tool to stage this in the channel. Never auto-send.
```

## Decision rules

- **Draft, never send.** The personal agent never calls the send-message tool. The team-facing agent stages the draft and the human sends from the UI. This is the only way to keep the wall intact.
- **Substantive ships first.** If the team only reads the first three bullets, they should still get the most important news.
- **"Recent context" is a scalpel, not a bucket.** Use it only when an older item is live-ammo for today or tomorrow. A week-old decision surfacing as context is fine; surfacing it because the section exists is noise.
- **"Waiting on" with names and since-when.** Vague blockers ("waiting on procurement") produce no action. Named blockers ("waiting on Sam at Acme on the procurement signoff since Apr 17") produce nudges.
- **Enrich with contributor commits if your team convention supports it.** A common pattern is for the team-facing agent to run `git log --since="1 day ago" --all` and add any teammate commits not already on the list. This captures team movement that your personal layer missed.

## Anti-patterns

- **Auto-send.** Never. Even if the team chat is private. Even if you trust the draft. Draft, review, send manually.
- **Leaking personal-layer names.** If a name from your personal layer (an internal agent handle, a working-title for a project that has a different public name) appears in the broadcast, the wall is broken. Re-draft.
- **Hype.** "Game-changing," "excited to announce," "thrilled." Not in this broadcast. Boring and credible beats creative and cringe.
- **Binary strawman.** "This is not just X, it is Y." State what it IS.
- **Dashes.** "to" or a comma or a reworded sentence. No em, no en.
- **Terse when the team wants long.** If your team has explicitly asked for long-form, respect it. Do not generic-brevity-rule their preference.

## Voice and filter guardrails

Before finalizing, run through this checklist:

1. Your voice, in the tense and person the channel expects.
2. No personal-layer agent names or internal handles.
3. No em or en dashes.
4. No invented-foil binaries.
5. No hype words.
6. Ownership tags on every bullet.
7. "Waiting on" items have names and since-when.
8. Length matches your team's stated preference.

## Handoff

When Pass 4 completes, the drop file has all four sections populated. The personal agent session is done. The team-facing agent reads the drop file on the next trigger ("pick up today's EOD drop" or equivalent) and executes the commits, copies the staged daily update file into place, and stages the Slack draft for human review.
