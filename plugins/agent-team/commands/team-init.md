---
description: 'Wire the current repo to the agent team — pins the plugin, installs CLAUDE.md and the permissions baseline'
argument-hint: "[project name]"
---

Set this repository up to use the `agent-team` plugin as its single source of truth. Do all of it,
then report exactly what you wrote.

## 1. Pin the plugin (this is the part that makes it work on every machine)

Create or merge `.claude/settings.json` in the repo root so it contains:

```json
{
  "extraKnownMarketplaces": {
    "mhogild-agents": {
      "source": { "source": "github", "repo": "mhogild/claude-agent-team" }
    }
  },
  "enabledPlugins": { "agent-team@mhogild-agents": true }
}
```

Merge into the existing JSON — never clobber keys that are already there, especially `permissions`.
This file **must be committed**; it is what provisions the team for anyone who clones the repo.

## 2. Install the orchestrator guide

Copy `${CLAUDE_PLUGIN_ROOT}/templates/CLAUDE.md` to the repo root as `CLAUDE.md`, replacing
`{{PROJECT_NAME}}` with `$1` — or, if no argument was given, with the repo's directory name.

If a `CLAUDE.md` already exists, do **not** overwrite it. Show the user a diff of what the template
adds and let them choose: merge, replace, or skip.

## 3. Install the permissions baseline

Merge `${CLAUDE_PLUGIN_ROOT}/templates/settings.json` (its `permissions.allow` / `ask` / `deny`
arrays) into the repo's `.claude/settings.json`. Union the arrays; keep any repo-specific entries
already present; do not duplicate.

## 4. Verify and report

- Run `claude plugin list` and confirm `agent-team` resolves.
- Add `.claude/settings.local.json` to `.gitignore` if it isn't already ignored.
- Tell the user: the files you wrote, that a **restart is required** for the plugin to load, and
  that the next step is `/agent-team:new-project` (no `ROADMAP.md` yet) or `/agent-team:spec`
  (roadmap exists).

Never copy the agents or skills themselves into the repo. Copies are what cause version drift
between machines — the plugin is the only place they live.
