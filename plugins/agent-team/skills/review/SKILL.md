---
name: review
description: 'Phase 5 — the gate a built unit/phase passes before it reaches a human as "done": independent diff review, then a real smoke test of the rendered artifact, then a cold-readable PR body.'
---

# review

**Fresh window.** Input: the build diff. Output: review notes, verified smoke-test evidence, a PR
body. Fifth phase — see `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md` (or run `/agent-team:team-map`). This phase orchestrates the two standing gates;
the detail lives in `code-review-rubric` (applied by `agents/code-reviewer.md`) and
`verify-changes` (applied by `agents/verifier.md`).

## The sequence (do not reorder)

1. **Code review — independent, diff-only, scoped.** An independent code-reviewer reviews the
   diff, even changes the orchestrator wrote inline. That "it's small, I wrote it, it's just
   presentation" exception is the single most recurring process failure this team has had — do
   not take it. Apply the `code-review-rubric`; correctness on any customer- or leadership-facing
   number (its math *or* its presentation) is the dimension we've actually been burned on.
2. **Verify — real smoke test of the rendered artifact.** Green unit tests are necessary, not
   sufficient. The verifier loads the actual deployed/rendered result — including one real sign-in
   when identity/auth changed — and checks it against the SPEC's acceptance criteria. A coder must
   not self-certify a unit that touches the identity/auth or payment path.
3. **PR body — cold-readable.** The verifier drafts it once verified: what changed and why, what
   was verified and how (with the smoke-test evidence), what was deliberately left out of scope.

## Done means done

A "done" / "PROVEN end-to-end" marker requires the **human's confirmation of the rendered
artifact** — not a green suite, not a successful data write. If a gate can't run right now (usage
limit, no environment), the unit is presented as **"review pending,"** explicitly, or narrowed to
a scoped diff-only check of just the new surface — never silently promoted to done.
