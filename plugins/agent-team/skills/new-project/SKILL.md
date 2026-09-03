---
name: new-project
description: 'Level 1 — define the whole product once and break it into a buildable roadmap. Produces PROJECT.md (vision, scope, constraints) and ROADMAP.md (ordered features), then hands each feature to /spec. Run by product-manager with tech-lead on architecture.'
---

# new-project

**Runs in its own context.** Spawned as a subagent by the autonomous driver, or invoked directly.
Input: your product idea (a conversation). Output: `PROJECT.md` and `ROADMAP.md`
at the repo root. Owner: `agents/product-manager.md`, with `agents/tech-lead.md` for the high-level
architecture. This is the **top** of the pipeline — see `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md` (or run `/agent-team:team-map`).

## Where this sits

There are three levels. This skill is Level 1 — the whole product, defined **once** (re-run only
for a major pivot or a new milestone). It does **not** design or build anything; it decides *what
the product is* and *in what order it gets built*. Each roadmap item then flows down through
Level 2 (`/agent-team:spec → /research → /plan → /build → /review → /checkpoint`) as its own bounded slice.

Do not try to specify the whole product in one `/agent-team:spec` — that's the overload this level exists to
prevent. `/agent-team:spec` is per-feature; `/agent-team:new-project` is what feeds it a queue of features.

## Steps

1. **Interview the human** (this is context-gathering, so ask, don't assume): the problem and why
   now, the target users and their jobs, the must-have capabilities, explicit **non-goals**, and
   the hard constraints (compliance/regulatory, offline capability, target devices, tenancy, data
   residency — whatever is load-bearing for this product). This step is the human's *input*, not a
   gate: if they aren't in the loop, take the brief you were given, resolve load-bearing unknowns
   with a cheap probe, and record the rest as stated assumptions in `PROJECT.md`
   (`ESCALATION.md` §2–§3) rather than stalling.
2. **Write `PROJECT.md`** — the **product vision** (the world you're trying to create, and the
   customer problem it solves), the **primary target persona** named concretely, in-scope
   capabilities, non-goals, key constraints, and high-level success criteria. High-signal, not a
   business plan. Assert it yourself and move — the roadmap derives from it, and a vision waiting
   for a signature is a project not started (`ESCALATION.md` §2).

   State the problem before the solution. If `PROJECT.md` describes a system rather than a problem
   somebody has, the roadmap under it will be a feature list with nothing to judge it against.

   Optionally sketch a **Business Model Canvas** here — segments, value prop, channels, revenue,
   costs — to check the thing hangs together commercially. **Timebox it to one sitting.** It's a
   thinking aid, not a maintained artifact.
2b. **Then set the first milestone's focus** with `/agent-team:product-strategy`, which turns the
   vision into two or three business objectives with measurable key results. `ROADMAP.md` orders
   *opportunities to pursue*, not features promised — anything on it still has to survive
   `/agent-team:discovery` before it is specced.
3. **Sketch the high-level architecture** with tech-lead — the major components and boundaries,
   enough to inform ordering (not a detailed design). Capture it as a Mermaid diagram seed for
   `docs/ARCHITECTURE.md` (see `skills/diagrams/SKILL.md`); solution-lead fleshes it out at the
   first checkpoint.
4. **Write `ROADMAP.md`** — the product broken into an **ordered** list of features/milestones,
   each one a future `/agent-team:spec` target. Sequence by dependency and risk: foundational + top-risk
   integrations early (retire a named payments/auth risk with a real spike before features that
   depend on it). Put the **"▶ you are here"** marker on the first item.

## Exit gate & handoff

`PROJECT.md` is written and asserted, `ROADMAP.md` lists ordered features with the marker set.
Then **spawn a fresh-context subagent for the first roadmap item and continue** — do not stop and
ask the human to start a new session (`ESCALATION.md` §5). Say plainly what you handed off:
*"Project defined. Handing `<name>` to `/agent-team:spec` in a fresh context."* From here the
per-feature loop owns delivery; solution-lead keeps ROADMAP.md true at each
`/agent-team:checkpoint`, and product-manager re-runs this skill only for a new milestone or a pivot.
