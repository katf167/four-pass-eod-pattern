# EOD Drop Example: Thu 2026-05-14

This is a fully fictional worked example that demonstrates the four passes end to end. Org, people, projects, numbers, and dates are invented. The shape of the output is faithful to how a real drop file reads; none of the specifics map to any real customer, client, or revenue figure. Pattern as it runs, not my day.

For readability, I have kept the author voice and the Slack ownership tags under my own name, so you can see how I would write the file myself if I worked at the fictional Northbrook Studio. Everything about Northbrook is invented; I do not work there.

Setup for this example:

- **Fictional org:** Northbrook Studio, a 5-person product design consultancy. Not a real company.
- **Personal agent layer:** a Cowork session with file access, Gmail, Calendar, and Slack connectors. Runs daily todo tracking, drafts, and planning.
- **Shared repo:** a private GitHub repo called `team-os`, containing a rolling audit trail (`daily/updates/`), CRM (`company/crm.md`), program state (`programs/*.md`), and team profiles (`team/*.md`).
- **Team channel:** a private Slack channel called `#team-ops` where the daily broadcast lands.
- **Three active projects for this fictional example:** Design Sprint Workshop Series (a paid multi-week cohort), Sprint Playbook Handbook (a standalone self-study product), and Corporate Pipeline (an enterprise platform-redesign lead).

---

# Drop File

## 1. Canonical-state changes scanned

| # | Category | Summary | Source | Target path | Urgency |
|---|----------|---------|--------|-------------|---------|
| 1 | Contact | New contact: J. Park, first paying website customer, purchased the Sprint Playbook Handbook at the lower tier ($29). | Completed items in personal checklist. | `company/crm.md` (People table) | Normal |
| 2 | Pipeline | Design Sprint Workshop Cohort 1 registration closed at 12 seats paid. | Project file `design-sprint-workshop.md` in personal layer. | `programs/design-sprint-workshop.md` | Normal |
| 3 | Decision | Set a public pricing convention for the Sprint Playbook Handbook: $29 personal use, higher tier (price TBD) for institutional use. | Decision logged in personal layer today. | `programs/sprint-playbook.md` | Normal |
| 4 | Financial | Cohort 1 instructor invoice settled: $1,200 total revenue, 50/50 split with teammate R. | Personal revenue tracker. | `company/financials.md` | Normal |
| 5 | External activity | Press mention: Design Sprint Workshop referenced in a design-industry newsletter roundup. | Email thread today. | `daily/updates/` narrative only; no separate file. | Normal |
| 6 | Waiting-on | Corporate Pipeline lead: still no procurement confirmation from primary contact S. Martinez at Acme Platform; 5-day lag since last nudge. | Personal checklist "Waiting On" section. | Note in Pass 3 "Waiting on" section. | Normal (escalation scheduled Fri May 22 if still silent) |
| 7 | Policy | Locked two standing rules for the reinforcement-email series: (a) no individual participant names in email body, (b) no instructor personal anecdotes unless directly relevant to the lesson. | Feedback from today's email send. | `programs/design-sprint-workshop.md` (Standing rules section) | Normal |
| 8 | Time-sensitive | None today. |

## 2. Commit manifest

### Commit 1: CRM contact add (PR required)

**Path:** `company/crm.md`
**Change type:** Update (add row to People table).
**Change description:** Add J. Park as a new contact; first paying website customer; product = Sprint Playbook Handbook ($29 personal use tier).
**Push style:** PR required (cross-team content).
**Reviewer:** shared-repo contributor R.
**Commit message:** `crm: add J. Park, first paying handbook customer`
**Proposed content:**

```
| J. Park | jpark@example.com | Independent | Self-study customer | 2026-05-14 | First paying website customer; personal-use tier ($29) |
```

### Commit 2: Design Sprint Workshop enrollment update (direct to main)

**Path:** `programs/design-sprint-workshop.md`
**Change type:** Update (Enrollment section).
**Change description:** Set Cohort 1 paid enrollment to 12 and mark registration closed.
**Push style:** Direct to main (my own program file; numeric status update only).
**Commit message:** `design-sprint-workshop: Cohort 1 enrollment final = 12, registration closed`
**Proposed content (replace):**

```
### Cohort 1 (opens Sat May 16, 2026)
- Paid enrollment: 12 (closed 2026-05-14)
- Instructor: teammate R.
- Revenue: $1,200 gross, 50/50 split with instructor.
- Format: live cohort-based, 4 weeks reinforcement follow-on.
```

### Commit 3: Sprint Playbook pricing convention (PR required)

**Path:** `programs/sprint-playbook.md`
**Change type:** Update (Pricing section).
**Change description:** Document the two-tier pricing convention for the Sprint Playbook Handbook. Personal use $29 (live); institutional use TBD price, product card to follow.
**Push style:** PR required (pricing is cross-team canonical state).
**Reviewer:** shared-repo contributor R.
**Commit message:** `sprint-playbook: document personal vs institutional pricing tiers`
**Proposed content (append to Pricing section):**

```
## Pricing tiers

**Personal use:** $29. Single-seat, self-study license, no redistribution.
**Institutional use:** price TBD. Multi-seat, internal-training license.

Product cards and checkout paths for both tiers live on the website.
First paying customer under the personal-use tier: 2026-05-14.
```

### Commit 4: Financial actuals (direct to main)

**Path:** `company/financials.md`
**Change type:** Append (Revenue log).
**Change description:** Log Cohort 1 gross revenue and instructor split.
**Push style:** Direct to main (append-only log).
**Commit message:** `financials: log Cohort 1 revenue and instructor split`
**Proposed content (append):**

```
| 2026-05-14 | Design Sprint Workshop Cohort 1 | $1,200 | 50/50 | $600 to teammate R., $600 to Northbrook |
```

### Commit 5: Design Sprint Workshop standing rules (PR required)

**Path:** `programs/design-sprint-workshop.md`
**Change type:** Append (Standing rules section).
**Change description:** Lock two rules learned from today's reinforcement send: no individual participant names in body, no instructor personal anecdotes unless lesson-relevant.
**Push style:** PR required (program rules affect future team sends).
**Reviewer:** shared-repo contributor R.
**Commit message:** `design-sprint-workshop: lock participant-naming and anecdote rules`
**Proposed content (append):**

```
## Standing rules (locked 2026-05-14)

1. **Naming rule.** No individual participant names in the email body. Program-wide references only.
2. **Personal anecdote rule.** Instructor personal anecdotes appear only when directly relevant to the lesson's topic. Family or outside-work anecdotes should not appear in program emails.

Both rules apply to all future reinforcement-series sends.
```

### Daily update file

Path: `daily/updates/2026-05-14-kat.md`
Push style: direct to main (my own daily update file).
Content: see Section 3 below.

## 3. Daily update file (staged)

**Path:** `daily/updates/2026-05-14-kat.md`

```
# 2026-05-14 daily update, Kat

## Design Sprint Workshop
Cohort 1 registration closed today at 12 paid seats. Gross revenue is $1,200 with a 50/50 instructor split, so $600 each to teammate R. and the studio. Kickoff is Saturday May 16. The full reinforcement-email series runs starting Mon May 18; Week 1 send is queued.

Today's send of the Week 1 email also produced two standing rules that now apply to every future send in the series. First rule: no individual participant names in the email body. Second rule: no instructor personal anecdotes unless they land directly on the topic being taught. Both rules are documented in the program file and will be audited on every future draft.

## Sprint Playbook Handbook (self-study product)
First paying website customer today: J. Park, who purchased at the $29 personal-use tier. Delivery sent the same day. We now have a two-tier pricing convention on file, with $29 for personal use and institutional use priced TBD. The institutional product card is the next build.

## Corporate Pipeline
Primary contact S. Martinez at Acme Platform has been silent 5 days on procurement confirmation. Nudge checkpoint is Fri May 22; walk-away decision is Tue May 26 if still silent. Pricing at $5,000 and scope are already confirmed, so only procurement is outstanding. If the pipeline closes lost, the May 26 calendar hold releases and the line comes off our projections.

## External activity
The Design Sprint Workshop was referenced today in a design-industry newsletter roundup. Full thread went to the core contributors. This is the first press surface for this workshop program.

## Coming up
- Sat May 16, 2026: Design Sprint Workshop Cohort 1 kickoff session (teammate R. leads).
- Mon May 18, 2026: Week 1 reinforcement email sends to the cohort.
- Fri May 22, 2026: Corporate Pipeline procurement nudge checkpoint.
- Tue May 26, 2026: Corporate Pipeline walk-away decision if still silent.

## Waiting on
- Corporate Pipeline primary contact: procurement confirmation. Silent since May 9. Nudge scheduled May 22.

## Notes
- Cohort 1 is the first cohort under the current pricing and split structure. The revenue-split model will be re-evaluated after three cohorts (target Aug).
- The standing rules locked today are a direct output of the personal-layer QA process. They are the kind of thing the shared repo needed on paper and did not have until today.
```

## 4. Team broadcast draft

Channel: `#team-ops`. Post under Kat's name.

```
*Shipped today*
- Design Sprint Workshop Cohort 1 registration closed at 12 paid seats. $1,200 gross, 50/50 split with R. ($600 each). Kickoff Sat May 16. (Kat)
- First paying Sprint Playbook Handbook customer: J. Park ($29 personal-use tier). Delivery sent same day. (Kat)
- Sprint Playbook pricing convention on file: $29 personal use, institutional use TBD. Institutional product card is next. (Kat)
- Design Sprint Workshop standing rules locked: no participant names in body, no instructor personal anecdotes unless lesson-relevant. Applies to all future sends. (Kat)
- Design Sprint Workshop press pickup: referenced in a design-industry newsletter roundup. Full thread shared with core contributors. (Kat)

*Coming up*
- Sat May 16: Design Sprint Workshop Cohort 1 kickoff (R. leads).
- Mon May 18: Week 1 reinforcement email to cohort.
- Fri May 22: Corporate Pipeline procurement nudge.
- Tue May 26: Corporate Pipeline walk-away decision.

*Waiting on*
- Corporate Pipeline primary contact S. Martinez at Acme Platform: procurement confirmation. Silent since May 9.
```

---

## Notes on this example

A few things to notice about the real output versus a generic template:

1. **Volume varies by day.** This was a substantive day. Quiet days produce a much shorter drop file. Sometimes just one commit, a three-line daily update, and a two-bullet broadcast.
2. **Pass 3 does not echo Pass 2.** Commit 5 (standing rules) is a structured commit. The daily update narrates the same information as two sentences inside the Design Sprint Workshop section. The broadcast summarizes it in one bullet. Same content, three granularities.
3. **"Waiting on" with since-when earns its place.** Corporate Pipeline's procurement silence is the kind of item that gets attention because the 5-day lag and the May 26 walk-away are both in the bullet.
4. **Time-sensitive items would have surfaced earlier.** None today. If there had been one (a new contact the team would ping tomorrow, a policy change affecting work in flight), it would have surfaced inline mid-day per the protocol in Pass 1, not waited for this batch.
5. **Push-style distinctions are load-bearing.** Commits 2 and 4 go direct to main because they are my own files. Commits 1, 3, and 5 require PR because they touch cross-team state. Without this distinction, either the review surface collapses (everything direct) or the day's work stalls (everything PR).
