---
description: 'Print the agent team''s workflow map — which phase you''re in, when to open a fresh window, how to seed it'
---

Read `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md` and `${CLAUDE_PLUGIN_ROOT}/ESCALATION.md`, and answer the
user's question from them. `ESCALATION.md` outranks `WORKFLOW.md` wherever they disagree about
stopping for the human — describe the pipeline as it actually runs, not as the phase files' older
phrasing might read.

If they asked nothing specific, summarise: the three levels, the phase pipeline table, and the
single next command that fits the repo's current state (check for `PROJECT.md`, `ROADMAP.md`, and
`.planning/*/` to work out where they are). Do not paste the whole file back.

## Describe the pipeline as agent-driven

The phases are **handoffs between agents, not a queue of human approvals.** When you describe the
pipeline:

- A phase boundary is where a **fresh-context agent is spawned**, not where the run pauses for the
  user. "Phase done — open a new window" is the old model; the team continues on its own.
- The gates in the pipeline are **agent-run and still binding**: independent code review of every
  diff including anything the orchestrator wrote inline, a verifier that smoke-tests the real
  rendered artifact rather than re-reading green tests, contract-foundation-first before parallel
  units, spikes ahead of their dependents, and "review pending" rather than a silent "done" when a
  gate could not run. Autonomy removed the human's approvals; it did not remove these, and it made
  them the only thing between a mistake and the record.
- The team **stops for the user only on an `ESCALATION.md` §1 trigger** — security in substance,
  secrets, prod/money/real people, irreversible or outward-facing, legal interpretation with
  liability, phishing/abuse — plus one named exception: `retro` never edits a team-definition file
  without explicit approval, because that is self-modification.
- Everything else is decided and logged in `.planning/DECISIONS.md`, which the end-of-run report
  links.

If the user asks where a human gate went, or asks you to confirm something the doctrine says the
team decides, point them at `ESCALATION.md` and its §5 anti-pattern table rather than re-inventing
an answer. Don't offer to add a checkpoint back in.
