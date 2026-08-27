# Changelog

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
