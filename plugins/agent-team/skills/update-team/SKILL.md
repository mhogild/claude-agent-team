---
name: update-team
description: 'Update the agent team to the newest published version, check which version this machine is on, or install the team on a new PC. Use when the user asks to update, upgrade, sync, or refresh the agent team / agents / skills, asks what version of the team they are running, or asks how to get the team onto another machine.'
---

# update-team

The team ships from **<https://github.com/mhogild/claude-agent-team>** as the `agent-team` plugin
in the `mhogild-agents` marketplace. It is **pull-based and manual** — nothing syncs on its own,
and nothing ever pushes back from a consuming machine.

## Updating this machine

Run both commands, in this order. The first refreshes the marketplace catalog; without it the
second can report "already at the latest version" against a stale catalog.

```bash
claude plugin marketplace update mhogild-agents
claude plugin update agent-team
```

Then report to the user:

- The version transition the CLI printed (e.g. `updated from 1.0.0 to 1.1.0`), or that they were
  already current.
- **That they must restart Claude Code for it to take effect.** Say this plainly and every time —
  an updated plugin does nothing until the next session starts. Do not present the update as
  finished without it.

If it says "already at the latest version" but the user believes they pushed a change, the cause is
almost always a **missing version bump** — see below. Check with `claude plugin list` (installed
version) against the `version` field in the repo's two manifests.

## Installing on a new machine

```bash
claude plugin marketplace add mhogild/claude-agent-team
claude plugin install agent-team@mhogild-agents
```

Restart, then verify with `claude plugin list` — expect `agent-team@mhogild-agents`, `Scope: user`,
`Status: enabled`. Scope `user` means every project on that machine gets the team, including a brand
new empty folder. The marketplace repo is public, so no GitHub auth is involved.

To wire a specific repo so it declares the team for future machines, run `/agent-team:team-init`.

## Publishing a change to the team

If the user wants to *change* an agent or skill, the edit belongs in the source repo — never in the
installed copy under `~/.claude/plugins/cache/`, which is a version-pinned cache that is replaced on
the next update.

1. Edit in the `claude-agent-team` working tree.
2. **Bump `version` in BOTH manifests** — `.claude-plugin/marketplace.json` and
   `plugins/agent-team/.claude-plugin/plugin.json`. They must agree.
   **This step is not optional:** `claude plugin update` compares the declared *version*, not the
   commit. A push without a bump is invisible to every other machine, and reports success while
   doing nothing. This is the single most common way the machines drift apart.
3. `claude plugin validate plugins/agent-team` — catches malformed YAML frontmatter, which
   otherwise loads a skill with all metadata silently dropped.
4. Commit and push. Optionally `claude plugin tag plugins/agent-team` to cut a release tag.
5. Update each machine with the two commands above, and restart each.
