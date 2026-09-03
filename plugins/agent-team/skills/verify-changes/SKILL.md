---
name: verify-changes
description: 'The gate a build unit must pass before it''s presented as done — a real smoke test of the rendered/deployed result, not a re-read of green unit tests.'
---

# verify-changes

Reconstructed from `.planning/TEAM-EVAL.md` (episode 2026-07-24) and
`dashboard/.planning/TEAM-EVAL.md` (episode 2026-07-30). See `README-PROVENANCE.md`. See also
`agents/verifier.md`, which runs this gate.

## The gate

**[attested]** Smoke the deployed preview, including one real sign-in, whenever the unit touches
identity/auth. Unit tests passing is necessary, not sufficient — a 403 admin-identity bug reached
a human's live sign-in once specifically because this step was skipped and only unit tests had
run.

**[attested]** A "PROVEN end-to-end" or "done" marker requires the **verifier's** confirmation of
the **rendered artifact itself** — the verifier loading the real thing, not a curated-data write,
not a green test suite. Twice, "PROVEN end-to-end" was declared and committed before anyone had
actually loaded the result and found it materially wrong (empty dashboard; wrong leadership
number). The gate exists to make that sequencing impossible: verify the rendered thing, *then*
declare done, never the reverse.

The human used to be the one who loaded it. They no longer are (`ESCALATION.md` §2), so this gate
is the whole of the protection that used to be shared — **it binds harder, not softer**
(`ESCALATION.md` §6).

## If you can't run it

**[attested]** A blocked verify step (usage limit, no environment access) is not license to
promote the unit to done anyway. Hold it as "review pending," explicit and visible, or narrow to
a scoped diff-only check of just the new surface. "Review pending" is a real state that survives
into the end-of-run report — never a silent "done."
