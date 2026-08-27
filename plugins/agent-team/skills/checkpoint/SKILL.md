---
name: checkpoint
description: 'Phase 6 — make the durable record match git reality so a newcomer could resume from disk alone. Run by solution-lead at the end of a phase (and again whenever a gated unit lands late).'
---

# checkpoint

Input: git reality since the last checkpoint. Output: updated `ROADMAP.md` / phase `SUMMARY.md` /
`README.md` / diagrams. Owner: `agents/solution-lead.md`. Sixth phase — see `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md` (or run `/agent-team:team-map`).
This one doesn't need a fresh window; it runs at the end of a phase's window.

## The standard every checkpoint is held to

**A newcomer could resume from disk alone.** If `ROADMAP.md`'s "▶ you are here" marker, the phase
`SUMMARY.md`, the `README.md`, and any diagrams don't reflect what was actually built and shipped
— including a unit that landed *after* the phase was nominally done — the checkpoint has failed,
even if the code is fine.

## Sequence

1. Confirm **every commit since the last checkpoint** is reflected in ROADMAP / SUMMARY / README /
   diagrams. Cross-check against `git log`, don't trust memory.
2. Move the **"▶ you are here"** marker.
3. Keep the **README** a true description of what the project currently *is* — not stale
   boilerplate or a description of a platform the project has since migrated off.
4. **Commit the checkpoint deliverable itself** before taking on new work. Atomic-commit
   discipline that holds for code but lapses for the checkpoint is exactly backwards — the
   checkpoint is the thing meant to make state legible.

## Keep the visual docs true

The record includes the **pictures**: `docs/ARCHITECTURE.md`, `docs/ERD.md`, `ROADMAP.md`, and any
key sequence diagrams — all Mermaid. At each checkpoint, confirm they still match what was actually
built (cross-check the schema and wiring against `git`), or update them via
`skills/diagrams/SKILL.md`. A stale ERD or architecture diagram fails the same
newcomer-resumes-from-disk standard as a stale SUMMARY.

## Re-checkpoint on late landings

Re-run whenever a previously-blocked unit lands, not only at full-phase end. A phase can read
"done" in the docs while a gated unit is still landing days later; the record has to catch up then
too.
