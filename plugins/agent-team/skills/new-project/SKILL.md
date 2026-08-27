---
name: new-project
description: 'Level 1 — define the whole product once and break it into a buildable roadmap. Produces PROJECT.md (vision, scope, constraints) and ROADMAP.md (ordered features), then hands each feature to /spec. Run by product-owner with tech-lead on architecture.'
---

# new-project

**Fresh window.** Input: your product idea (a conversation). Output: `PROJECT.md` and `ROADMAP.md`
at the repo root. Owner: `agents/product-owner.md`, with `agents/tech-lead.md` for the high-level
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
   residency — whatever is load-bearing for this product). Resolve load-bearing unknowns now — a cheap question beats a wrong roadmap.
2. **Write `PROJECT.md`** — vision/problem, users, in-scope capabilities, non-goals, key
   constraints, and high-level success criteria. High-signal, not a business plan. Get the human's
   explicit sign-off; the roadmap derives from it.
3. **Sketch the high-level architecture** with tech-lead — the major components and boundaries,
   enough to inform ordering (not a detailed design). Capture it as a Mermaid diagram seed for
   `docs/ARCHITECTURE.md` (see `skills/diagrams/SKILL.md`); solution-lead fleshes it out at the
   first checkpoint.
4. **Write `ROADMAP.md`** — the product broken into an **ordered** list of features/milestones,
   each one a future `/agent-team:spec` target. Sequence by dependency and risk: foundational + top-risk
   integrations early (retire a named payments/auth risk with a real spike before features that
   depend on it). Put the **"▶ you are here"** marker on the first item.

## Exit gate & handoff

`PROJECT.md` is signed off, `ROADMAP.md` lists ordered features with the marker set. Then tell the
human plainly: *"Project defined. Start a fresh session and run `/agent-team:spec` for the first roadmap item —
`<name>`."* From here the per-feature loop owns delivery; solution-lead keeps ROADMAP.md true at
each `/agent-team:checkpoint`, and product-owner re-runs this skill only for a new milestone or a pivot.
