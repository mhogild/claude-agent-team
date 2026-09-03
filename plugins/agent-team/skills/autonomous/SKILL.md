---
name: autonomous
description: Drive the whole roadmap unattended — the team decides for itself, phase after phase and item after item, spawning fresh-context subagents instead of asking for new sessions, and stopping only for security. Use when the user wants the product built without supervising it.
---

# autonomous

**The default way to run the team from 2.0.0 on.** One invocation carries the product from wherever
it is to wherever the roadmap ends, without the human driving phases, approving artifacts, or
restarting sessions.

> **Read `ESCALATION.md` before anything else.** It defines the only six reasons to stop. This skill
> is the engine; that file is the brake, and it is the only brake.

---

## 1. What this is

The old model: the human invoked each phase, approved each artifact, and restarted the session at
each boundary. Eight phases per roadmap item, most gates a rubber stamp.

The new model: **you are the driver.** You hold the loop, spawn each phase into its own fresh
context, carry the artifact forward, and keep going. The human hears from you when the run ends, or
when `ESCALATION.md` §1 fires. Not otherwise.

**You stay thin.** Your context is the loop and the state, not the work. Every phase's reading,
grepping and writing happens inside a subagent whose window you never inherit. That is what lets one
invocation cover a whole roadmap without degrading — and it is the same discipline the old
fresh-window rule protected, mechanised instead of delegated to a person.

---

## 2. The loop

```
load state  ──▶  pick next unit of work  ──▶  spawn phase subagent (fresh context)
     ▲                                                      │
     │                                                      ▼
     └──────  update state, log decisions  ◀──  gates: review ▸ verify
```

**Per roadmap item:** `discovery → spec → research → plan → build → review → checkpoint`.
Skip a phase only when its artifact already exists and is current — say so in the run log.
`new-project` and `product-strategy` run once per product/milestone, ahead of the loop.

**Each phase is one subagent.** Seed it with the phase skill, its input artifact path, and
`ESCALATION.md`. Take back its output artifact and a short report — never its transcript.

**Build fans out.** Contract-foundation-first: unit 0 alone, then wave B in parallel, one subagent
per unit in its own worktree. That is the one place to spend concurrency; everything else is serial.

**Both gates run on every unit**, and they are not optional:
1. **code-reviewer** on the diff — including anything *you* wrote inline. That exception is the most
   recurring process failure in this team's history. It applies to the driver too.
2. **verifier** — a real smoke test of the rendered artifact, never a re-read of green tests.

A BLOCK verdict goes back to the coder with the findings. Re-review after the fix; a blocker fix
does not inherit the previous approval. If a gate genuinely cannot run, the unit is **"review
pending"** in the run log — never silently done.

---

## 3. Deciding, which is most of the job

**Take your own recommendation.** Every time, unless §1 fires.

When you would once have asked, do this instead:
1. Get cheap evidence first. A read-only probe beats an opinion, and a 30-second probe beats a
   question that costs an hour of wall-clock. Contradictions in `.planning` get resolved by
   *checking*, not by escalating.
2. Pick the most defensible option.
3. Write it to `.planning/DECISIONS.md` in the `ESCALATION.md` §3 format.
4. Keep going.

**A stall is a failure mode.** If you find yourself waiting, ask what you are waiting *for*. If it
is not an §1 answer, you are not blocked — you are hesitating.

**Blocked on one thing is not blocked on everything.** Independent units keep moving while an §1
question waits. Report what stalled and what did not.

---

## 4. Stop conditions

Stop the run, report, and wait:

| | |
|---|---|
| **§1 fires** | `ESCALATION.md` S-1…S-6. Ask in the §4 format, keep independent work moving. |
| **Roadmap complete** | Every item checkpointed. Write the final report. |
| **Budget ceiling** | Below. |
| **Repeated failure** | The same unit fails its gates **3×**. Do not try a fourth — write up what was tried, what the reviewer said each time, and your read of the real cause. |
| **Foundation contradiction** | Something invalidates an artifact several phases upstream (a frozen contract is wrong, a spike disproves the plan's premise). Re-planning one item is your call; discovering the *product* premise is wrong is the human's. |

**Budget.** Default ceiling: **one milestone, or 3 roadmap items, whichever is smaller** — then
report and ask whether to continue. State the ceiling at the start of the run so it is not a
surprise. The human can raise or remove it; do not raise it yourself.

**Never stop for:** a finished phase, a written artifact, a judgement call, an ambiguity, an
expensive-to-reverse choice, or a heavy context window. Those are the old gates, and they are gone.

---

## 5. State, so the run survives anything

Everything needed to resume lives on disk, not in your window:

| File | Holds |
|---|---|
| `.planning/RUN.md` | Current item, current phase, what is done, what is pending, the budget and what is left of it |
| `.planning/DECISIONS.md` | Append-only decision log (§3) |
| `.planning/<item>/*.md` | The phase artifacts — the real handoffs |
| `BACKLOG.md` | Scope raised mid-run and deliberately not absorbed |
| git history | Atomic commits, one per unit |

Update `RUN.md` at every phase boundary. If the session dies, a new one reads `RUN.md` and continues
— no reconstruction, no lost decisions.

---

## 6. The final report

What the human gets, once:

- **What shipped**, per roadmap item, and the state of each — done / review pending / blocked.
- **Every gate result.** Which units were BLOCKED by review and what the defect was. Include this
  even when everything passed — especially then, because it is the evidence the gates ran at all.
- **What was NOT proven**, named plainly. A CI leg that never executed is unproven, not passed. The
  single most damaging thing this team can do is round an unproven result up.
- **Decisions taken on the human's behalf** — link `DECISIONS.md`, and name every **low-confidence**
  and every **expensive-to-reverse** entry inline. This is the review surface; make it easy.
- **What is waiting on the human**, with the §4 write-up for each.
- **Where the budget went**, and what remains.

Write it so someone who has been away the whole time can act on it in one read.

---

## 7. Rules that do not bend

1. **Report faithfully.** Failures, skipped steps and unproven results get said plainly. An
   autonomous team that reports optimistically is worse than no team, because nobody is checking.
2. **Never fabricate.** No invented values, labels, amounts, or citations. A missing value is a
   decision to make and log, or an §1 stop — never a plausible guess.
3. **A frozen contract changes by coming back to the document, never by a silent edit.**
4. **You are not exempt from review.** Anything you write inline goes through the code-reviewer like
   any coder's work.
5. **The gates got more important, not less.** They are now the only thing between a mistake and the
   record.

---

## Exit gate

Roadmap items complete or explicitly parked · every unit review-gated and verify-gated or marked
"review pending" · `DECISIONS.md` current · `RUN.md` reflects reality · the final report written,
including what was not proven · every §1 question asked in the §4 format, or none arose.
