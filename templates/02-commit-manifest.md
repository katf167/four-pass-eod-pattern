# Pass 2: Commit Manifest

**Runs in:** personal agent session
**Writes to:** Section 2 of the drop file
**Blocks on:** Pass 1 output
**Feeds:** Pass 3 (daily update)

## Purpose

Turn the flagged changes from Pass 1 into a precise, actionable list of repo edits. Each entry is a proposed commit: which file, what change, what the new content should look like, and how the change should be pushed.

The personal agent drafts. The team-facing agent executes later. This separation is load-bearing: the personal agent does not hold repo credentials, and the team-facing agent does not see personal-layer content except through the drop file.

## Inputs

1. Pass 1's flagged changes.
2. The shared repo's file tree and current file contents, read-only.
3. Your repo's push style rules (direct to main vs PR required, by content type).

## Outputs

One entry per proposed commit. Each entry includes:

- **Path:** the repo-relative file path.
- **Change type:** create, update, append, replace, delete.
- **Change description:** one sentence on what is changing and why.
- **Proposed content:** the exact new text, or a before-and-after diff sketch for partial edits. Markdown inline if short; separate block if long.
- **Push style:** direct-to-main or PR. See repo rules.
- **Reviewer:** if PR, who should review.

## Prompt skeleton

```
Run Pass 2 of the Four-Pass EOD Pattern.

For each item in Section 1 of the drop file at {drop file path}:
  1. Identify the target file in the shared repo at {shared repo path}
  2. Decide change type (create, update, append, replace, delete)
  3. Draft the exact new content or a clear before-and-after sketch
  4. Apply the repo's push-style rules:
       {paste push-style rules here, e.g.:
        - Your own daily update files: direct to main.
        - Personal folder contents: not pushed (gitignored).
        - Your own profile file: direct to main for minor self-edits.
        - Anything in company/, programs/, or cross-team files: PR required.
        - Files others authored: PR required.
        - Shared infrastructure (CLAUDE.md, .gitignore, configs): PR required.
       }
  5. If PR, name the expected reviewer.

Write each entry under "## 2. Commit manifest" in the drop file. One entry per commit.

Do not execute any git operations. Drafting only.
```

## Decision rules

- **One commit per logical change.** Do not batch unrelated edits. The team-facing agent will need to author a separate commit message for each, and the review surface is clearer when changes are atomic.
- **Default to PR for cross-team content.** If the edit touches files others authored, policies, shared configs, or anything whose rollback would require a postmortem, PR is the right choice. Direct to main is reserved for your own content.
- **If a change requires reading a file you have not read, read it first.** Drafting an update without reading the current state produces edits that either conflict or duplicate.
- **Include a one-line commit message proposal.** Short, imperative, scoped to the edit. This is the only narrative output from Pass 2; the full narrative goes in Pass 3.
- **If the content is long, write it as a fenced block.** Inline is fine for a one-line update. Anything over five lines gets its own block so the team-facing agent can copy it cleanly.

## Anti-patterns

- **Over-drafting.** Writing a full essay for a one-line status change. Match the weight of the edit.
- **Skipping push style.** Without a push-style decision, the team-facing agent has to re-decide each commit. Leaving that to execution time produces inconsistent repo hygiene.
- **Inventing paths.** If the target path does not exist in the shared repo, say so explicitly and propose the new path. Do not assume; the team-facing agent should not have to verify every path.
- **Bundling.** Combining two unrelated edits into one commit so the manifest looks shorter. Atomic commits are worth more than a short manifest.

## Push-style rules (template)

Customize these for your repo. A common starting point:

**Direct to main acceptable for:**
- Your own daily update file at `daily/updates/YYYY-MM-DD-{you}.md`
- Contents of your personal workspace folder (gitignored)
- Typo fixes to your own content
- Minor self-edits to your own profile file

**PR required for:**
- Shared infrastructure (`CLAUDE.md`, `.gitignore`, root configs)
- Cross-team content (shared folders: `company/`, `programs/`, shared contacts)
- Files authored by other contributors
- New folder structures or convention changes
- Anything whose rollback would need a postmortem

**Always:**
- Work on a feature branch for PR-required changes
- Never force-push, reset, or delete shared branches without explicit alignment
- When in doubt, open a PR

## Handoff

When Pass 2 completes, the drop file has Section 2 populated. Pass 3 reads Section 1 and Section 2, then writes the rolling audit trail entry.
