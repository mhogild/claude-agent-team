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
adds and let them choose: merge, replace, or skip. (This one is not a workflow gate — it is the
user's own file in their own repo, and clobbering it is destructive and unasked-for.)

## 2b. Install escalation awareness

The team is **autonomous by default**. Make sure the repo's `CLAUDE.md` points at
`${CLAUDE_PLUGIN_ROOT}/ESCALATION.md` and says what it does: it outranks every phase skill and every
agent definition, it names the six cases the team stops and asks about (security, secrets,
prod/money/real people, irreversible or outward-facing, legal interpretation with liability,
phishing/abuse), and it says the team decides everything else and logs it. If the template already
carries that pointer, confirm it survived the merge; if the user chose "skip" or "merge" and it did
not land, say so plainly in your report — a repo wired to the team without the escalation doctrine
will fall back to asking about everything.

Create `.planning/DECISIONS.md` if it doesn't exist, with just its heading. It is where every
decision that would once have been a human gate gets logged, append-only, and it is the human's
after-the-fact review surface.

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
- Set expectations about how the team now runs, in a sentence or two: **it drives itself.** Once
  started it moves through the pipeline, spawning fresh-context agents per phase, approving its own
  SPEC/PLAN/RESEARCH and having its own reviewer and verifier check them. It comes back to the user
  for an `ESCALATION.md` §1 case, and otherwise at the end of the run, with a report that links
  `.planning/DECISIONS.md` and names every low-confidence and expensive-to-reverse call it made.
  Say this explicitly — a user expecting a gate after every phase will read autonomy as the team
  ignoring them.

Never copy the agents or skills themselves into the repo. Copies are what cause version drift
between machines — the plugin is the only place they live.
