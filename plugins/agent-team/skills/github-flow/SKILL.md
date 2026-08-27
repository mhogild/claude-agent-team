---
name: github-flow
description: 'Conventions for pushing, PRs, and CI once a project is on GitHub — added the day this team''s project first reached GitHub and got a working CI gate.'
---

# github-flow

Reconstructed from `dashboard/.planning/SESSION-2026-08-13.md` and
`dashboard/.planning/TEAM-EVAL.md` (episode 2026-08-13). See `README-PROVENANCE.md`.
This skill is new relative to the rest of the team — added the same session that first pushed
the project to GitHub and got CI running.

## PR bodies

**[attested]** `verifier` drafts the PR body once a unit/phase is confirmed (see
`agents/verifier.md`) — not the coder, not the orchestrator by default.

## PR description standard

**[inferred — "the worked example of the new PR-description standard" is attested, its content
is not]** Write the PR description so a reviewer who wasn't in the session can evaluate it cold:
what changed and why, what was verified and how, what was deliberately left out of scope. Treat
this the same way `coder.md`'s "state what you did NOT do" applies to unit reports — the PR body
is the outward-facing version of that same discipline.

## CI as a real gate, not decoration

**[attested]** The first time CI actually ran on this project, it caught three real defects in
23 seconds that had been invisible for 152 commits — including a contract-drift check that could
only ever pass on one specific machine (a Node version mismatch between local and CI silently
made the guard a no-op everywhere else). Lesson: a check that only ever runs successfully in one
environment is not verified — confirm your CI actually exercises the same command/scope your
local checks do (this generalizes `coder.md`'s CI-scope rule to the pipeline definition itself,
not just to what a coder runs locally).

## Permissions

**[attested, fact only — not reconstructed]** This same session made the three-tier
(allow/ask/deny, shape-matched pattern) permissions config global, in `~/.claude/settings.json`.
The actual patterns aren't recoverable from evidence; rebuild this by using Claude Code normally
in the new project and letting the allow/ask/deny prompts accumulate, or copy the real file from
the work PC once reachable.
