---
name: plan
description: 'Phase 3 — turn SPEC + RESEARCH into a precise PLAN with build units, frozen contracts, and verification steps. Run by tech-lead. Runs in its own context; the second high-leverage gate.'
---

# plan

**Runs in its own context.** Spawned as a subagent by the autonomous driver, or invoked directly.
Input: `SPEC.md` + `RESEARCH.md`. Output: `.planning/<feature>/PLAN.md`.
Owner: `agents/tech-lead.md`. Third phase — see `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md` (or run `/agent-team:team-map`).

## Why precision here pays off most

A bad line of a plan can cause hundreds of bad lines of code. This is the second high-leverage
gate — now a reviewer's, not a human's (`ESCALATION.md` §2). The plan is where intent (SPEC) and
facts (RESEARCH) become an executable, verifiable decomposition. Aim for a tight plan a reviewer
can evaluate cold — a 200-line plan beats a 2000-line surprise PR.

## The plan contains

- **Build units** — each a unit of work assignable to one coder/frontend-engineer in a fresh
  worktree. For every unit: its `spec-ref` (which acceptance criteria it satisfies), the files it
  owns, and its frozen contract/interface.
- **Contract-foundation-first.** If units run in parallel, land the shared contract/schema stub as
  **unit 0** — empty but structurally real — before parallel units start. Give each coder an
  isolated worktree with zero shared production files.
- **Integration-risk spikes.** Any named top risk from RESEARCH (auth/payments/SDK) gets a real
  end-to-end spike scheduled **before** the units that depend on it — not a docs read.
- **Verification steps** — how each unit and the phase as a whole will be smoke-tested against the
  SPEC's acceptance criteria.
- **Sequence & dependencies** — what must land before what.

## Exit gate

PLAN.md exists, every unit carries a spec-ref and a contract, and spikes are sequenced ahead of
their dependents. tech-lead asserts this gate and proceeds; the reviewer verifies the assertion.
**Contract-foundation-first and spikes-ahead-of-dependents are not negotiable** — they are
agent-to-agent gates, and they bind harder now that no human approval sits behind them
(`ESCALATION.md` §6). Only a plan meeting them proceeds to `/agent-team:build`.
