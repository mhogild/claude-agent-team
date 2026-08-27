# Agent team — workflow map

**Owner:** solution-lead · **Updated:** 2026-08-15 · **Scope:** how to run this agent team.
**Model:** Cagan / SVPG - empowered product team, four big risks, outcome over output.
**Source of truth:** <https://github.com/mhogild/claude-agent-team> - never edit a copy inside a consuming repo.

This is the one-page answer to *"which phase am I in, when do I open a fresh context window,
and how do I seed it?"* Read it when you're unsure what to run next.

## Why phases exist (the one idea)

The context window is the only lever on output quality. Every phase below exists to do one
thing: **compact high-volume, low-signal work (file reads, greps, tool output) into a small
durable artifact, then start the next phase in a fresh window seeded by that artifact.** Keep
utilization roughly in the 40–60% band — when a window gets heavy, checkpoint to disk and
restart clean rather than pushing on in a degraded context.

Human review is cheapest and highest-leverage *early*: a bad line of research can cause
thousands of bad lines of code; a bad line of a plan, hundreds. So the human gates are on
**spec, research, and plan** — not mainly on the final diff.

## Three levels

Know which level you're at before picking a command:

- **Level 1 — product** (once): the whole product → `/agent-team:new-project` → `PROJECT.md` + `ROADMAP.md`.
- **Level 1.5 — strategy** (per milestone): `/agent-team:product-strategy` → focus, target persona,
  business objectives with measurable key results.
- **Level 2 — opportunity** (per roadmap item): the pipeline below, `/agent-team:discovery` through
  `/agent-team:checkpoint`.
- **Level 3 — build unit**: one coder's slice, assigned by tech-lead inside `/agent-team:build`.

`/agent-team:spec` is per-opportunity, not per-product. No `ROADMAP.md` yet? Start with
`/agent-team:new-project`.

**The one idea behind the shape.** A team handed a list of features to build is a *feature team*,
accountable for shipping. A team handed a problem to solve is an *empowered product team*,
accountable for the result. Everything from `spec` rightward is delivery and was always strong here;
`product-strategy` and `discovery` exist so the team is deciding what deserves to be built, and
`checkpoint` closes the loop by asking whether it worked. Skip those and this is a very well-run
feature factory.

## The pipeline

| Phase | Command | Fresh window? | Reads (input) | Writes (output) | Human gate |
|-------|---------|:---:|---------------|-----------------|:---:|
| New project | `/agent-team:new-project` | yes | your product idea (once) | `PROJECT.md` + `ROADMAP.md` | ✅ sign off vision + roadmap |
| Strategy | `/agent-team:product-strategy` | yes | `PROJECT.md` (per milestone) | `.planning/STRATEGY.md` (focus, persona, objectives + key results) | ✅ approve strategy |
| Discovery | `/agent-team:discovery` | yes | `STRATEGY.md` + a candidate problem | `.planning/<opportunity>/DISCOVERY.md` (verdict: validated / killed / pivoted) | ✅ approve verdict |
| Spec | `/agent-team:spec` | yes | a **validated** `DISCOVERY.md` | `.planning/<feature>/SPEC.md` (+ acceptance criteria, DoR) | ✅ approve spec |
| Research | `/agent-team:research` | yes | `SPEC.md`, the codebase | `.planning/<feature>/RESEARCH.md` | ✅ review findings |
| Plan | `/agent-team:plan` | yes | `SPEC.md` + `RESEARCH.md` | `.planning/<feature>/PLAN.md` (units, contracts, tasks) | ✅ approve plan |
| Build | `/agent-team:build` | yes (per phase) | `PLAN.md` | code + atomic commits | — |
| Review | `/agent-team:review` | yes | the build diff | REVIEW notes, verified smoke test, PR body | ✅ preview rendered artifact |
| Checkpoint | `/agent-team:checkpoint` | no (end of phase) | git reality + `DISCOVERY.md` predictions | `OUTCOME.md` (did the key result move?) + updated ROADMAP / SUMMARY / README / diagrams | — |
| Retro | `/agent-team:retro` | yes | session transcript + commits | `TEAM-EVAL.md` entry (+ maybe a team-def fix) | ✅ approve any team-def change |

Non-build work (conversation, config, strategy, planning-doc surgery) runs under `/agent-team:catchup`
instead of this pipeline — see `skills/catchup/SKILL.md`.

## How to seed a fresh window (the convenient part)

When a phase says "fresh window," that means: **start a new Claude Code session** and open it by
naming the phase and pointing at the input artifact. For example:

- Research: *"Run `/agent-team:research` for `.planning/checkout-flow/SPEC.md`."*
- Plan: *"Run `/agent-team:plan` for checkout-flow — inputs are its SPEC.md and RESEARCH.md."*
- Build: *"Run `/agent-team:build` from `.planning/checkout-flow/PLAN.md`."*

Each phase skill states its own input/output up top, so you don't have to remember them. You do
**not** need to keep the previous phase's window open — the artifact on disk is the handoff.

## Who does what (roster)

| Role | Owns | File |
|------|------|------|
| orchestrator | the main session; routes phases, spawns subagents | — (main session) |
| product-manager | the *what & why*: spec, acceptance criteria, backlog, Definition of Ready | `agents/product-manager.md` |
| tech-lead | the *how*: decomposition into units, frozen contracts, integration-risk spikes, build gates | `agents/tech-lead.md` |
| coder | one backend/logic unit in an isolated worktree, against a frozen contract | `agents/coder.md` |
| frontend-engineer | one UI unit — design-system, accessibility, responsive discipline | `agents/frontend-engineer.md` |
| code-reviewer | independent scoped diff review before human preview | `agents/code-reviewer.md` |
| verifier | real smoke test of the rendered/deployed artifact; drafts the PR body | `agents/verifier.md` |
| solution-lead | the durable record — ROADMAP / SUMMARY / README / diagrams stay resume-able from disk | `agents/solution-lead.md` |
| retro | scores each episode and (only on a recurring pattern + your OK) fixes a team-definition file | `agents/retro.md` |

Roster discipline: multi-agent work costs ~15× the tokens of a single chat and only pays off on
genuinely parallel, high-value work. Don't spawn a subagent where a phase artifact + one worker
will do. The team is deliberately small; grow it only when a real recurring need shows up in
`/agent-team:retro`, not on a hunch.

## Definition of Ready (before `/agent-team:build` touches a unit)

- `SPEC.md` is approved, with written acceptance criteria.
- `RESEARCH.md` and `PLAN.md` exist and were reviewed.
- Any named top integration risk (auth/payments/SDK) has a completed real end-to-end spike.
- The shared contract/schema stub exists (or is unit 0 of this build).

## Definition of Done (before "done" is spoken)

1. Independent code-reviewer diff review has run — even on changes the orchestrator wrote inline.
2. Verifier ran a real smoke test of the rendered artifact (not a re-read of green unit tests).
3. The human has confirmed the rendered artifact. If a gate can't run, it's **"review pending,"**
   explicitly — never silently promoted to done.
