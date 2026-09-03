---
name: catchup
description: 'Lighter-weight lane for non-build sessions — conversation, config, strategy stress-tests, history surgery — where the full build workflow doesn''t apply, but core team rules still must.'
---

# catchup

Reconstructed from `dashboard/.planning/TEAM-EVAL.md` (episode 2026-08-13) and
`dashboard/.planning/SESSION-2026-08-13.md`. See `README-PROVENANCE.md`.

## §1 — Announce the lane

**[attested, 2026-08-13 addition]** Say explicitly, at the start of the session, that you're
running in `catchup`, not `build`. This matters mechanically, not just for clarity: rules that
live inside `build/SKILL.md` do not fire automatically for a `catchup` session, and the team has
already lost real time to exactly that gap once (see §2).

## §2 — Inherit build's retract-then-proceed rule

**[attested]** `build/SKILL.md`'s retract-then-proceed rule (raise a concern, retract it, then
say so explicitly and drop it rather than silently carrying it forward) is graduated and
enforced — but it lives in `build`, and one `catchup` session burned ~26% of its length on
exactly the failure that rule exists to prevent, because the session was running `catchup` and
the rule never fired. **This rule applies here too, explicitly, regardless of which skill you're
nominally running.** Don't assume a lane-specific skill silently inherits every cross-cutting
team rule — check.

## §3 — `ESCALATION.md` applies here too

**[inferred, by the same logic as §2]** `catchup` is the lane most likely to drift into asking the
human, because it *is* the conversational lane — the human is right there. That is not a licence.
`ESCALATION.md` outranks this skill as it outranks every other: stop for §1, decide everything
else, log it per §3. A question that would be a failure of nerve in `build` is still one here.

## When to use catchup vs. build

**[inferred]** Use `catchup` for work that doesn't produce build units to hand to a coder:
strategic stress-testing a premise, config/permissions changes, planning-doc reconciliation,
one-off investigation. If the session is going to produce code changes assigned as discrete
units with review/verify gates, that's `build`, not `catchup` — don't let a session drift from
one into the other without noticing (see §1).
