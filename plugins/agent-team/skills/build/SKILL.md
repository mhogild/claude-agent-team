---
name: build
description: 'Main delivery workflow — frame the work, assign units to coders, gate on review/verify, checkpoint the durable record. Use for any real build task, as opposed to catchup/conversational work.'
---

# build

Reconstructed from `.planning/TEAM-EVAL.md`, `dashboard/.planning/TEAM-EVAL.md` (all episodes),
and `dashboard/.planning/SESSION-2026-08-13.md`. See `README-PROVENANCE.md`.

## Place in the pipeline (added 2026-08-15)

**Fresh window.** Input: an approved `.planning/<feature>/PLAN.md`. Output: code + atomic commits.
`build` is the **implement** phase — phase 4 of the pipeline in `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md` (or run `/agent-team:team-map`)
(spec → research → plan → **build** → review → checkpoint). Do not start `build` on a feature
whose PLAN.md hasn't been reviewed and whose Definition of Ready isn't met (see
`agents/product-manager.md` and `agents/tech-lead.md`). Each build unit carries a `spec-ref` back to
the acceptance criteria it satisfies, so review and verify can check the built thing against what
was actually asked for.

## §Frame

**[attested]** Before scope locks, resolve any contradiction already sitting in `.planning`
state that would change what gets built. Do **not** classify it as "an open item, not a
blocker" and carry it forward unresolved — that pessimistic-default move cost real time twice:
once when stale planning docs drove wrong provisioning steps, and once when a flagged
RBAC-access contradiction was logged, marked not-a-blocker, and the team spent ~2 hours building
against the pessimistic assumption before a single 30-second probe (`az account show`) proved it
wrong. **Ask the human, or run a cheap read-only empirical probe, before scope locks — every
time a `.planning` contradiction would change the build.**

## §Contract-foundation-first

**[inferred, pattern name attested]** When a phase has multiple coders working in parallel: land
the shared contract/schema stubs as unit 0, *empty but structurally real*, before parallel units
start. Give each subsequent coder an isolated worktree with zero shared production files. This
was called out as the best-executed comms pattern observed — collisions were limited to
predicted shared-test files, resolved as a union, with zero production-file conflicts.

## Review and verify gates

**[attested]** A unit or phase is not "done" until:
1. An independent code-reviewer diff-review has run — see `code-reviewer.md`. This applies
   **even to changes the orchestrator wrote inline** — that exception has been the single most
   recurring process failure across episodes (3 separate occurrences before it was fixed).
2. A verifier has actually run — a real smoke test of the deployed/rendered result, not a re-read
   of unit tests. See `verifier.md`.
3. If either gate can't run right now (usage limit, session end), the unit is presented to the
   human as **"review pending"**, explicitly — never silently promoted to done.

**[attested]** A "done"/"PROVEN end-to-end" marker specifically requires the **human's
confirmation of the rendered artifact** — not a passing test suite, not a successful data write.
This was violated twice, each time with the human discovering the real state (a blank dashboard,
a wrong number) themselves, after "done" had already been declared.

## §Retract-then-proceed (lines 39–43 in the original)

**[attested — this section number/line range is directly cited in the evidence]** If you raise a
concern or an alarm about something, and then decide it doesn't actually apply, **say so
explicitly and drop it from your plan** — do not silently keep carrying the retracted item
forward as if it were still live. Failing to do this once cost ~26% of an entire session pursuing
a self-raised, self-retracted, never-actually-relevant risk.

**[attested, gap found 2026-08-13]** This rule only fires inside the `build` skill by default.
Sessions running under `catchup` need it too — see `catchup/SKILL.md` §2, where this exact
failure recurred because the session wasn't running `build`.

## Scope discipline

**[attested]** A feature request or scope change raised mid-build does not get folded into the
current unit's scope. Capture it in `BACKLOG.md` with its open design questions, confirm any
ambiguous semantics with the human, and keep building the originally scoped unit. This pattern
held cleanly across every episode it was tested.

## Dev/prod isolation

**[attested]** The human runs anything that touches production credentials or infrastructure
directly (Azure/SWA provisioning, schema changes, `az` sign-in). When an agent must run a
**read-only** call against production (e.g. confirming a real number the human explicitly asked
to see), that still gets a one-line explicit confirmation first — *"this will hit prod read-only
with your session — confirming"* — even under a direct, unambiguous instruction to proceed. Never
assume permission silently, even when it would obviously be granted.
