---
name: retro
description: 'The self-correcting loop — after an episode, score it against evidence and, only on a pattern that recurred ≥2×, apply one human-gated, logged fix to a team-definition file. Merges the former team-review skill.'
---

# retro

**Fresh window.** Input: the session transcript + the episode's commits. Output: a
`.planning/TEAM-EVAL.md` entry, and — sometimes — one logged edit to a team-definition file.
Run by `agents/retro.md`. See `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md` (or run `/agent-team:team-map`). This replaces the former `team-review` skill.


## Score the outcome, not only the process

Before scoring how the episode ran, ask what it produced. Pull the `OUTCOME.md` entries closed since
the last retro and check:

- How many shipped opportunities **moved the key result they predicted**?
- How many were **never instrumented**, so nobody can say?
- How many discoveries ended in **killed** or **pivoted**? A team that never kills an idea is not
  testing them — it is validating decisions already made, and its discovery phase is decoration.

Persistent "shipped, never measured" or a kill rate of zero are patterns worth the one gated
team-definition fix this skill allows — they mean the team has drifted back into being a delivery
factory, which no amount of process scoring will surface.

## Step 1 — Locate evidence (before anything else)

Find the transcript that actually covers the episode and **confirm its time span overlaps the
episode's commit dates** (`git log`) before scoring. The pending-episode marker has pointed at a
stale or unrelated transcript more than once; treat "the marker might be wrong" as the default,
and key off the transcript that actually contains the episode's commits.

## Step 2 — Score

Score 1–5 on the five fixed dimensions — **outcome, intent (incl. SPEC acceptance-criteria
fidelity), comms, process/gate adherence, efficiency/context hygiene** — and write the entry:
one-line episode description, per-dimension scores, the single weakest link with evidence, what
went well (with a note not to "tune away" working behaviour), then findings as
**evidence → minimal fix → target file**, each tagged one-off or `[RECURRING ≥2×]`. If there was
no delegation, adapt the comms dimension explicitly rather than scoring it as a failure.

## Step 3 — Log or graduate

Every finding is logged. A finding becomes an actual team-definition edit **only** when the same
underlying issue has recurred **≥2×** across separate episodes, **and** the human explicitly
approves. Collapse multiple overlapping one-off notes into a single graduated rule rather than
applying several redundant edits — the target file should get more precise over time, not longer.
Every applied edit gets a dated `docs/team-changelog.md` entry (target file, exact addition, why).

## Trigger

Run at natural episode boundaries — end of a build phase, a live cutover, a standalone
investigation — not on a timer. Batching to the next natural boundary is fine as long as it runs
before the evidence goes stale.
