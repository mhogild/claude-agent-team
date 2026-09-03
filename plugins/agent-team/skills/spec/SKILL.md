---
name: spec
description: 'Phase 2 - turn a validated opportunity into a SPEC with testable acceptance criteria and the outcome it must move, before any research or code. Run by product-manager. Runs in its own context.'
---

# spec

**Runs in its own context.** Spawned as a subagent by the autonomous driver, or invoked directly.
Input: a **validated** `DISCOVERY.md` (or a written note saying why discovery was
skipped). Output: `.planning/<feature>/SPEC.md`. Owner: `agents/product-manager.md`. Second phase — see `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md` (or run `/agent-team:team-map`).

Spec-driven development is not outdated — it's how you agree on *what* before spending tokens on
*how*. Community data on this pattern reports large drops in rework versus prompt-driven building.
The spec is the source the plan's contracts and the verifier's checks both derive from.

You are writing this **after** the idea survived `/agent-team:discovery`, so do not re-open whether
to build it. Spec turns a validated opportunity into something buildable and checkable.

## Steps

0. **Trace the outcome.** Name the business objective and key result this serves, and link the
   `DISCOVERY.md` verdict. If discovery was skipped, state why in one line (no value risk: bug fix,
   compliance obligation, refactor, direct human request). A spec that cannot name the result it is
   supposed to move is an output with no outcome — send it back to discovery.
1. State the **problem and why now** in a few sentences, from the customer's side. Derive it from
   `DISCOVERY.md` and `STRATEGY.md`; if it genuinely isn't there, write the gap down as an
   explicit assumption — don't invent a rationale and present it as fact.
2. Write **acceptance criteria** as a numbered list, each one testable and observable in the
   rendered artifact. Write them the way the verifier will check them.
3. Write **out of scope** — what this explicitly does not cover.
4. List **open questions**. Resolve the load-bearing ones with a cheap read-only probe, or by
   taking the most defensible reading and stating the assumption in the SPEC — logged per
   `ESCALATION.md` §3. A `.planning` contradiction that would change what gets built is resolved
   now, not carried forward as "not a blocker."
5. **Assert the exit gate yourself and proceed.** The spec is the team's to approve — see
   `ESCALATION.md` §2. Write the assertion into the SPEC, hand it to `/agent-team:research`, and
   let the reviewer check the assertion. No approval commit, no ticked box, no waiting.

## Exit gate (Definition of Ready)

Asserted by product-manager, verified by the reviewer — not waited on. Traces to an objective and
key result · discovery verdict is *validated* (or the skip is justified in writing) · the four
risks are each retired-with-evidence or explicitly-accepted-in-writing-with-a-reason ·
acceptance criteria testable · out-of-scope stated · dependencies and named integration risks
flagged. Only then does the feature move to `/agent-team:research`.

## Keep it small

A spec is high-signal, not a requirements novel. If it's growing past a screen or two, you're
probably designing the solution — that's `/agent-team:plan`'s job, in a later phase's own context.
