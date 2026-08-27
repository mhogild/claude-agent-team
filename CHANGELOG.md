# Changelog

## 1.1.0 - 2026-08-27

Two behaviours that used to depend on a per-repo CLAUDE.md now travel with the plugin itself.

- **SessionStart hook** injects the orchestrator protocol into every session on the machine: announce
  the lane, and proactively tell the user when to start a fresh context window (at every phase
  boundary, and mid-phase past the 40-60% band). Self-limiting - it tells the model to ignore it in
  projects that do not use the team.
- **PreCompact hook** fires when the window is about to be compacted and turns that into a
  checkpoint: write the phase artifact, then tell the user to start a fresh session.
- **`/agent-team:update-team` skill** so "update the agent team" resolves to the real commands
  (`marketplace update` then `plugin update`), always prompts for the required restart, and documents
  that a push without a version bump is invisible to other machines.
- Hooks use `cat` on a static JSON payload - works unchanged under bash and PowerShell, no `jq` or
  `node` dependency.

## 1.0.1 - 2026-08-27

Docs only. Proves the update path: `claude plugin update` compares the declared
**version**, not the commit sha - a push without a version bump is invisible to
other machines.

## 1.0.0 — 2026-08-27

Extracted the agent team from `mhogild/POS-all-in-one` into its own marketplace + plugin so it is
portable across repos, GitHub accounts, and machines.

- 8 agents, 13 skills and `WORKFLOW.md` moved in unchanged except as noted below.
- Genericised the ~9 POS-specific lines (frontend-engineer device/accessibility notes, diagrams
  architecture example, new-project constraints prompt, workflow-map title).
- Namespaced every pipeline command reference to `/agent-team:<phase>`, removing the collision with
  the loose `~/.claude/commands/{build,plan,review,spec}.md`.
- Repointed `.claude/WORKFLOW.md` references to `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md`.
- Added `/agent-team:team-init` (pins the plugin + installs CLAUDE.md and the permissions baseline
  into any repo) and `/agent-team:team-map`.
- Added the orchestrator `CLAUDE.md` and permissions-baseline `settings.json` as templates.
