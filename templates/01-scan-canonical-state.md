# Pass 1: Scan Canonical-State Changes

**Runs in:** personal agent session
**Writes to:** Section 1 of the drop file
**Blocks on:** nothing
**Feeds:** Pass 2 (commit manifest)

## Purpose

Inventory everything that happened today in your personal layer that should update the shared repo's canonical state. Canonical state means the team-facing facts that persist beyond today: contacts, pipeline, program details, team roles, policies, decisions, status markers.

This pass is pure inventory. No drafting of edits yet. That is Pass 2.

## Inputs

1. Your personal layer's daily activity log. In my setup, that is a `CHECKLIST.md` with a "Completed today" section, plus any project files edited today.
2. The shared repo's current canonical state, read-only. The agent needs to see what is already there so it only flags real changes.
3. The "what crosses vs what stays" wall for your setup.

## Outputs

A list of flagged changes. Each entry includes:

- **Category:** contact, pipeline, program, team, policy, decision, status marker, financial, external activity, waiting-on.
- **Summary:** one sentence describing what changed.
- **Source:** which file or activity in the personal layer this came from.
- **Where it lands:** tentative repo path (e.g., `company/crm.md`, `programs/<name>.md`).
- **Urgency:** normal (batch at EOD) or time-sensitive (flag inline mid-day, see protocol below).

## Prompt skeleton

```
Run Pass 1 of the Four-Pass EOD Pattern.

Scan today's activity in the personal layer for canonical-state changes that should update the shared repo. Check:
  - Today's "Completed" items in {personal checklist path}
  - Project files edited today in {personal projects path}
  - New contacts added or existing contacts updated
  - Pipeline movement (new leads, stage changes, close-lost, close-won)
  - Program or curriculum state changes
  - Team role or policy changes
  - Financial actuals (revenue, invoices, payouts)
  - External activity worth logging (press, conferences, partnerships)

For each flagged change, write:
  1. Category
  2. One-sentence summary
  3. Source in the personal layer
  4. Tentative target path in the shared repo: {shared repo path}
  5. Urgency: normal or time-sensitive

Apply the "what crosses vs what stays" wall:
  {paste your wall here}

Skip items that stay in the personal layer. Skip items already present in the shared repo.

Write the output into the drop file at {drop file path} under the heading "## 1. Canonical-state changes scanned."
```

## Decision rules

- **If nothing crossed today, write `None today.` and stop.** EOD is opt-in, not automatic. Quiet days should produce a quiet drop file.
- **If a single item spans multiple categories, list it once under the most specific category.** Avoid duplicates; the commit manifest in Pass 2 handles multi-file changes.
- **If you are unsure whether something crosses, err toward flagging.** Pass 2 can drop it. Pass 1 should catch everything; false negatives are harder to recover from than false positives.
- **If an item is time-sensitive, flag it inline to the user before EOD.** Do not wait to batch it. See protocol below.

## Time-sensitive flag protocol

Not every canonical-state change can wait. A new contact the team will ping tomorrow, a pipeline decision the team needs before a morning meeting, a policy change that affects ongoing work: these surface the same day, inline, before EOD batch.

The personal agent flags mid-day in chat:

> Noticed a canonical-state change that may be time-sensitive: {summary}. Push now or batch at EOD?

The user decides. If push now, Pass 2 through Pass 4 run immediately for that item. If batch, the item waits for the normal EOD pass.

## Anti-patterns

- **Inventory sprawl.** Listing every todo completed today. Pass 1 is for canonical state, not activity log. "Sent 3 emails" does not cross. "New contact added" does.
- **Silent skips.** If you decide an item stays in the personal layer, do not drop it without noting the decision somewhere. Over time the personal layer should codify its wall; silent skips prevent that codification.
- **Pre-drafting the edits.** Pass 1 is inventory. If you start drafting commit content here, Pass 2 becomes redundant and the drop file becomes hard to review.
- **Skipping the read of the shared repo.** Without reading the current state, you cannot tell what is a real change. Always read-before-flag.

## Handoff

When Pass 1 completes, the drop file has Section 1 populated. Pass 2 reads that section and drafts the commit manifest.
