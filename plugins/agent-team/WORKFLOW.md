# Agent team — workflow map

**Owner:** solution-lead · **Updated:** 2026-09-03 · **Scope:** how to run this agent team.
**Model:** Cagan / SVPG - empowered product team, four big risks, outcome over output.
**Source of truth:** <https://github.com/mhogild/claude-agent-team> - never edit a copy inside a consuming repo.

> ## ⚠️ 2.0.0 — the team is AUTONOMOUS by default
>
> **Stop for security. Decide everything else.** The team drives its own pipeline, approves its own
> artifacts, and spawns its own fresh contexts. It stops only for the six cases in
> **`ESCALATION.md`** — security, secrets, prod/money/real people, irreversible or outward-facing
> actions, legal interpretation with liability, and abuse-shaped work.
>
> **`ESCALATION.md` outranks this file and every skill.** Wherever anything below still reads like a
> human approval gate, that file wins.
>
> Run **`/agent-team:autonomous`** to drive the whole roadmap unattended.

This is the one-page answer to *"which phase am I in, what runs next, and what does it read?"*

## Why phases exist (the one idea)

The context window is the only lever on output quality. Every phase below exists to do one thing:
**compact high-volume, low-signal work (file reads, greps, tool output) into a small durable
artifact, then start the next phase in a fresh context seeded by that artifact.** Keep utilization
roughly in the 40–60% band — when a window gets heavy, write the artifact and hand the rest to a
fresh subagent rather than pushing on in a degraded context.

**That "fresh context" is a spawned subagent, not a new session.** This is the one thing 2.0.0
changed about the mechanism: the discipline is identical, but the team provides its own clean
windows instead of asking a person to restart. Nobody is ever told "start a new session."

Review is cheapest and highest-leverage *early*: a bad line of research can cause thousands of bad
lines of code; a bad line of a plan, hundreds. Still true — but that is an argument for **reviewing
early, not for a *human* reviewing early.** The reviewer and verifier are agents, they run on every
phase artifact and every diff, and they are now the only backstop. That raises what they must catch;
it does not lower the bar.

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

**`/agent-team:autonomous` runs this entire table for you**, item after item, until the roadmap ends
or `ESCALATION.md` §1 fires. Invoke a single phase directly only when you want just that one thing.

| Phase | Command | Own context | Reads (input) | Writes (output) | Stops for human? |
|-------|---------|:---:|---------------|-----------------|:---:|
| New project | `/agent-team:new-project` | subagent | your product idea (once) | `PROJECT.md` + `ROADMAP.md` | **interviews you** — it needs what only you know |
| Strategy | `/agent-team:product-strategy` | subagent | `PROJECT.md` (per milestone) | `.planning/STRATEGY.md` (focus, persona, objectives + key results) | no |
| Discovery | `/agent-team:discovery` | subagent | `STRATEGY.md` + a candidate problem | `.planning/<opportunity>/DISCOVERY.md` (verdict: validated / killed / pivoted) | no — it may kill the idea itself |
| Spec | `/agent-team:spec` | subagent | a **validated** `DISCOVERY.md` | `.planning/<feature>/SPEC.md` (+ acceptance criteria, DoR) | no |
| Research | `/agent-team:research` | subagent | `SPEC.md`, the codebase | `.planning/<feature>/RESEARCH.md` | no |
| Plan | `/agent-team:plan` | subagent | `SPEC.md` + `RESEARCH.md` | `.planning/<feature>/PLAN.md` (units, contracts, tasks) | no |
| Build | `/agent-team:build` | subagent per unit | `PLAN.md` | code + atomic commits | only on §1 |
| Review | `/agent-team:review` | subagent | the build diff | REVIEW notes, verified smoke test, PR body | no — the **verifier** confirms the rendered artifact |
| Checkpoint | `/agent-team:checkpoint` | in the driver | git reality + `DISCOVERY.md` predictions | `OUTCOME.md` (did the key result move?) + updated ROADMAP / SUMMARY / README / diagrams | no |
| Retro | `/agent-team:retro` | subagent | session transcript + commits | `TEAM-EVAL.md` entry (+ maybe a team-def fix) | ✅ **yes** — editing the team's own rules is self-modification |

**Only two rows stop for you, and neither is an approval.** `new-project` *interviews* you because
the product idea is yours to give. `retro` gates because a team rewriting its own definition
unsupervised is a different risk class. Everything else decides and logs.

Non-build work (conversation, config, strategy, planning-doc surgery) runs under `/agent-team:catchup`
instead of this pipeline — see `skills/catchup/SKILL.md`.

## How a phase gets a clean context

**The driver spawns it.** Each phase runs in a subagent seeded with three things: the phase skill,
the path to its input artifact, and `ESCALATION.md`. It hands back its output artifact and a short
report — never its transcript, which is the whole point.

Every phase skill states its input/output signature in its header, so the driver never has to guess
what a phase reads. The artifact on disk is the handoff.

**Nobody is ever asked to start a new session.** If you find that instruction anywhere in this
plugin, it is stale — file it as a bug against 2.0.0.

## What the human actually does now

1. Says what to build, once.
2. Answers `ESCALATION.md` §1 questions if any arise — always with a full written explanation, never
   a diff and a checkbox.
3. Reads the end-of-run report, and reviews `DECISIONS.md` as a batch — after the fact, not as an
   interruption during.

That is the whole job.

## Who does what (roster)

| Role | Owns | File |
|------|------|------|
| orchestrator / driver | holds the loop; spawns each phase into its own context; never does the work itself | `skills/autonomous/SKILL.md` |
| product-manager | the *what & why*: spec, acceptance criteria, backlog, Definition of Ready | `agents/product-manager.md` |
| tech-lead | the *how*: decomposition into units, frozen contracts, integration-risk spikes, build gates | `agents/tech-lead.md` |
| coder | one backend/logic unit in an isolated worktree, against a frozen contract | `agents/coder.md` |
| frontend-engineer | one UI unit — design-system, accessibility, responsive discipline | `agents/frontend-engineer.md` |
| code-reviewer | independent scoped diff review; **also the §1 tripwire** | `agents/code-reviewer.md` |
| verifier | real smoke test of the rendered/deployed artifact; **confirms it**; drafts the PR body | `agents/verifier.md` |
| solution-lead | the durable record — ROADMAP / SUMMARY / README / diagrams stay resume-able from disk | `agents/solution-lead.md` |
| retro | scores each episode and (only on a recurring pattern + your OK) fixes a team-definition file | `agents/retro.md` |

Roster discipline: multi-agent work costs ~15× the tokens of a single chat and only pays off on
genuinely parallel, high-value work. Don't spawn a subagent where a phase artifact + one worker
will do. The team is deliberately small; grow it only when a real recurring need shows up in
`/agent-team:retro`, not on a hunch.

## Definition of Ready (before `/agent-team:build` touches a unit)

- `SPEC.md` exists with written, testable acceptance criteria, and its phase asserted its own exit
  gate. **Asserted by `product-manager`, verified by `tech-lead` at assignment — not human-approved.**
- `RESEARCH.md` and `PLAN.md` exist and passed their reviewer check.
- Any named top integration risk (auth/payments/SDK) has a completed real end-to-end spike.
- The shared contract/schema stub exists (or is unit 0 of this build).

## Definition of Done (before "done" is spoken)

1. Independent code-reviewer diff review has run — **even on changes the orchestrator wrote inline.**
2. Verifier ran a real smoke test of the rendered artifact (not a re-read of green unit tests).
3. **The verifier has confirmed the rendered artifact itself** — the actual page, flow or output as a
   person would meet it. The sequencing does not bend: verify the rendered thing, *then* say done,
   never the reverse. If a gate can't run, it's **"review pending,"** explicitly — never silently
   promoted to done.

**These are agent gates and they are not optional.** With the human out of the approval path they
are the only thing standing between a mistake and the record — more load-bearing in 2.0.0 than they
were in 1.x, not less.
