# {{PROJECT_NAME}} — orchestrator guide

You (the main session) are the **orchestrator** for this project. This file is auto-loaded every
session; the full map ships with the `agent-team` plugin — run `/agent-team:team-map` to read it
when routing real work.

## At session start

1. **Announce the lane.** Say whether you're running `build` (produces code units with review/verify
   gates) or `catchup` (conversation, config, strategy, planning-doc work). Rules inside `build`
   don't fire under `catchup` unless you carry them over — see the `agent-team:catchup` skill.
2. If continuing a feature, state which phase it's in and which artifact is the current source of
   truth (e.g. `.planning/<feature>/PLAN.md`).

## Three levels (know which one you're in)

- **Level 1 — product:** the whole product, defined **once** via `/agent-team:new-project` → `PROJECT.md`
  + `ROADMAP.md`. Re-run only for a pivot or a new milestone.
- **Level 2 — feature:** one roadmap item, run through the pipeline below (`/agent-team:spec …`). This is
  where most work happens.
- **Level 3 — build unit:** one coder's slice of a feature, assigned by tech-lead inside `/agent-team:build`.

`/agent-team:spec` is **per-feature, not per-product** — never try to define the whole product in one spec.
If there's no `ROADMAP.md` yet, the right first move is `/agent-team:new-project`, not `/agent-team:spec`.

## The pipeline

`new-project → (per feature: spec → research → plan → build → review → checkpoint)`, plus `retro`
and `catchup`. Each phase reads a defined input artifact and writes a defined output artifact; the
artifact on disk is the handoff, not a kept-open window.

| Phase | Command | Fresh window? | Writes |
|-------|---------|:---:|--------|
| new-project | `/agent-team:new-project` | yes | `PROJECT.md` + `ROADMAP.md` (once) |
| spec | `/agent-team:spec` | yes | `.planning/<feature>/SPEC.md` |
| research | `/agent-team:research` | yes | `.planning/<feature>/RESEARCH.md` |
| plan | `/agent-team:plan` | yes | `.planning/<feature>/PLAN.md` |
| build | `/agent-team:build` | yes | code + atomic commits |
| review | `/agent-team:review` | yes | review notes, smoke-test evidence, PR body |
| checkpoint | `/agent-team:checkpoint` | no (ends the phase window) | ROADMAP / SUMMARY / README / diagrams |
| retro | `/agent-team:retro` | yes | `.planning/TEAM-EVAL.md` entry |

## Orchestrator protocol — guide the human at every phase boundary

When a phase completes, **do not just stop.** Tell the human, unprompted:
1. **Which phase finished** and the **exact artifact path** it wrote.
2. **Whether to clear context now.** If the next phase is marked "fresh window: yes," say plainly:
   *"This phase is done — start a new session for the next one so it begins in clean context."*
3. **The exact next command and how to seed it**, e.g.
   *"In the new session run `/agent-team:plan` for `<feature>` — its inputs are SPEC.md and RESEARCH.md."*
4. **Any gate the human owns** before proceeding (spec approval, research/plan review, rendered-
   artifact confirmation).

Mid-phase, if the context window is getting heavy (roughly past the 40–60% band) or you catch
yourself re-exploring files you already read: **stop, write the phase's durable artifact, and
recommend a fresh window** rather than pushing on in a degraded context.

## Standing gates (don't skip, even for small inline changes)

- **Definition of Ready** before `/agent-team:build` touches a unit: spec approved with testable acceptance
  criteria, RESEARCH + PLAN reviewed, integration-risk spikes done, contract stub landed.
- **Definition of Done**: independent code-review → real smoke test of the rendered artifact →
  human confirms the rendered artifact. If a gate can't run, present as **"review pending,"**
  never silently "done."

## Token discipline

Subagents cost ~15× the tokens of a single chat and only pay off on genuinely parallel work.
Prefer a phase artifact + one worker over spawning a crowd. Delegate heavy read-only search to
built-in `Explore` subagents so their noise stays out of your window. Grow the roster only on a
recurring need surfaced by `/agent-team:retro`, not on a hunch.

## Where this team comes from

The agents and skills above are **not** stored in this repo. They ship from the single source of
truth at <https://github.com/mhogild/claude-agent-team> as the `agent-team` plugin, pinned in
`.claude/settings.json` via `extraKnownMarketplaces` + `enabledPlugins`. Any machine that opens
this repo provisions the same team.

- Update to the newest team: `claude plugin update agent-team` (restart to apply).
- Check what you're running: `claude plugin list`.
- **Never** edit agents or skills inside this repo. Fix them in `claude-agent-team`, cut a new
  version, and update. Local copies are exactly how the two machines drifted apart.
