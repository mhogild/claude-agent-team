# claude-agent-team

Single source of truth for the phase-gated Claude Code agent team — one repo, every project,
every machine, either GitHub account.

This repo is **both a Claude Code marketplace and the plugin it serves**. Nothing here is
project-specific; per-project content lives in each consuming repo.

## What's in the team

| | |
|---|---|
| **Agents (8)** | `product-owner` · `tech-lead` · `coder` · `frontend-engineer` · `code-reviewer` · `verifier` · `solution-lead` · `retro` |
| **Pipeline skills** | `new-project` → `spec` → `research` → `plan` → `build` → `review` → `checkpoint`, plus `retro` and `catchup` |
| **Support skills** | `code-review-rubric` · `verify-changes` · `diagrams` · `github-flow` |
| **Commands** | `/agent-team:team-init` · `/agent-team:team-map` |
| **Templates** | orchestrator `CLAUDE.md`, permissions baseline `settings.json` |

Everything is namespaced under `agent-team:`, so `/agent-team:build` never collides with a
`build` skill or command from anywhere else.

## Setup — once per machine

```bash
claude plugin marketplace add mhogild/claude-agent-team
claude plugin install agent-team@mhogild-agents
```

Works from any GitHub account and needs no auth: the repo is public.

## Setup — once per repo

In the repo, run:

```
/agent-team:team-init "My Project"
```

That writes `CLAUDE.md`, merges the permissions baseline, and — the part that matters — pins the
plugin in the repo's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "mhogild-agents": { "source": { "source": "github", "repo": "mhogild/claude-agent-team" } }
  },
  "enabledPlugins": { "agent-team@mhogild-agents": true }
}
```

Commit that file. Any machine that clones the repo now provisions the same team automatically —
no marketplace command needed on the new machine.

## Staying in sync

```bash
claude plugin update agent-team    # pull the newest team (restart to apply)
claude plugin list                 # see the version you're actually running
```

**The one rule:** never edit an agent or skill inside a consuming repo. Fix it here, bump
`version` in both `.claude-plugin/marketplace.json` and `plugins/agent-team/.claude-plugin/plugin.json`,
tag it, and update. Local copies are exactly how two machines drift apart.

```bash
claude plugin validate plugins/agent-team    # before you push
claude plugin tag plugins/agent-team         # cut agent-team--v<version>
```

## Layout

```
.claude-plugin/marketplace.json      the marketplace listing
plugins/agent-team/
  .claude-plugin/plugin.json         the plugin manifest
  agents/                            8 role definitions
  skills/                            13 phase + support skills
  commands/                          team-init, team-map
  templates/                         CLAUDE.md, settings.json
  WORKFLOW.md                        the workflow map (/agent-team:team-map)
```
