---
name: code-review-rubric
description: 'The correctness/reuse/efficiency dimensions a code-reviewer checks a diff against, including the hard-won leadership-facing-measure rule.'
---

# code-review-rubric

Reconstructed from `dashboard/.planning/TEAM-EVAL.md` (episodes 2026-07-29, 2026-07-30,
2026-07-30→31). See `README-PROVENANCE.md`. See also `agents/code-reviewer.md`, which applies
this rubric.

## Correctness

**[attested]** Standing rule, applied in its general form after recurring 3×:

> *"Any change to a leadership-facing measure — its math or its presentation — gets an
> independent scoped diff review before the human preview, including changes authored by the
> orchestrator."*

This rubric is now the **last** check before the record, not the second-to-last: the human no
longer approves the artifact (`ESCALATION.md` §2), and the orchestrator exception was already this
team's most recurring process failure. Anything identity, auth, permissions or secrets-shaped that
turns up in a diff is an `ESCALATION.md` §1 stop (S-1/S-2), not a review comment — flag it and
halt that unit.

**[attested]** Specific instance of this that's worth checking by name: for any
adoption/coverage/utilization-style ratio, verify the denominator is the real eligible/assigned
population, not merely users/events observed active in the period. This exact bug (a distinct-
users-in-period count used where an assigned-license count was needed) reached a leadership
dashboard once already.

## Reuse / simplification / efficiency

**[inferred — not separately evidenced beyond the correctness dimension]** Apply standard
reuse/simplification/efficiency review on top of correctness: flag unnecessary abstraction,
duplicated logic that could reuse an existing frozen contract or seam, and inefficient patterns —
but correctness on leadership-facing surfaces is the dimension this team has actually been burned
on, so don't let a clean simplification pass substitute for the correctness check above.

## Scope

**[attested]** Diff-only, scoped to the unit under review — not a full-codebase audit.
