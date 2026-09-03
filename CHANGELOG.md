# Changelog

## 2.0.0 - 2026-09-03

**The team is now autonomous.** It drives its own pipeline, approves its own artifacts, and spawns
its own fresh contexts. It stops for the human on security and almost nothing else.

The 1.x team was well-gated and slow to run. In practice its human gates split cleanly in two: the
**agent** gates (code review, verifier) caught real defects, while most **human** gates were rubber
stamps that cost a session restart each. A single real build phase under 1.2.0 required three
approval commits and four "start a new session" handoffs, and exactly one decision in it genuinely
needed a person.

### The rule

> **Stop for security. Decide everything else.**

- **New `ESCALATION.md`** — outranks every skill and agent definition. Defines the only six reasons
  to stop: **S-1** security in substance · **S-2** secrets and credentials · **S-3** production,
  money, or real people · **S-4** irreversible or outward-facing · **S-5** legal/regulatory
  *interpretation* with liability · **S-6** phishing, deception, abuse-shaped work. Everything else
  is decided and logged.
- **New `/agent-team:autonomous`** — drives the whole roadmap unattended, item after item, spawning
  each phase into its own context. Has a stated budget ceiling (default: one milestone or 3 items),
  explicit stop conditions, and `.planning/RUN.md` state so a dead session resumes from disk.
- **New `.planning/DECISIONS.md`** — append-only log of every judgement call that would once have
  been escalated, with the alternative not taken, the reversal cost, and a confidence rating. The
  end-of-run report names every low-confidence and expensive-to-reverse entry. **This is the human's
  review surface: a batch, after the fact, not an interruption during.**

### Approval gates removed

`spec`, `research`, `plan`, `discovery`, `product-strategy`, `new-project` no longer wait for a
human to approve their own output. Each phase asserts its exit gate; the reviewer verifies it.
Twelve approval gates removed, each leaving an `ESCALATION.md` pointer where it stood.

**Two stops kept, neither an approval.** `new-project` still *interviews* the human — the product
idea is theirs to give, not their approval of the team's output — but it no longer blocks if they
are absent. `retro` still gates team-definition edits: a team rewriting the rules that decide when
it stops, unsupervised, is a distinct risk class.

**Two stops added** (`ESCALATION.md` S-4): pushing to a public remote / opening a public PR
(`github-flow`), and publishing a team-definition change that lands on every machine
(`update-team`).

### Fresh sessions replaced by fresh subagents

The context discipline was right; the mechanism was manual. A phase still gets a clean window — it
just gets one by **being a subagent** rather than by asking a person to restart. Nine phase-skill
headers rewritten; input/output signatures preserved exactly, so the driver can consume them.
`hooks/session-start.json` and `hooks/pre-compact.json` rewritten — the pre-compact hook now says
checkpoint **and continue**, where it used to say stop.

### Agent gates strengthened, because they are now the only backstop

- `code-reviewer` gains the **§1 tripwire**: a diff touching auth, secrets, permissions, payments,
  PII or anything outward-facing that did *not* stop is raised as a finding. §1 is now enforced by
  an agent rather than by whoever is driving noticing.
- The **verifier's** confirmation of the rendered artifact replaces the human's. The sequencing is
  unchanged and does not bend: verify the rendered thing, *then* say done. "Review pending" still
  applies when a gate genuinely cannot run.
- **Definition of Ready** is asserted by `product-manager` and verified by **`tech-lead`** at
  assignment. Deliberately *not* `code-reviewer`, which is diff-only by design — judging whether
  four product risks carry real evidence is product judgement, and putting it on the diff reviewer
  is how a gate becomes a rubber stamp.
- `build` §Frame now says **probe first, always**; ask only when a probe is impossible *and* it is a
  §1 case.

### Breaking

- Consuming repos should re-run `/agent-team:team-init` to pick up the rewritten `CLAUDE.md`
  template, or the old fresh-window protocol will keep firing from their local copy.
- Anything that relied on a human approval commit per phase will no longer see one.

## 1.2.0 - 2026-08-27

Reshapes the team around Marty Cagan / SVPG: an empowered product team is handed a problem and is
accountable for the outcome, not handed features and accountable for shipping. Everything from
`spec` rightward was already strong delivery; what was missing was everything to its left, plus a
loop back at the end.

- **`product-owner` -> `product-manager`**, deliberately. "Product owner" is a Scrum backlog role
  and a small subset of product management; the name was keeping the job small. Now owns value and
  business viability and convenes all four risks.
- **New `product-designer` agent** completes the product trio (PM = value/viability, designer =
  usability, tech-lead = feasibility). Distinct from `frontend-engineer`, who builds the agreed
  interface - the designer decides what it should be, via throwaway prototypes.
- **New `/agent-team:product-strategy`** (Level 1.5, per milestone): focus and non-goals, insights,
  one target persona, business objectives that are qualitative with key results that are
  quantitative and business-level. Carries Cagan's own warning that OKRs are usually a waste and are
  a cultural mismatch for feature teams - use lightly or not at all.
- **New `/agent-team:discovery`** (Phase 1): opportunity assessment / customer letter framing,
  ideation across several candidates, risk-appropriate prototyping, and testing the four big risks.
  Ends in an explicit verdict - validated, killed, or pivoted. Killing ideas is the phase working.
- **`spec` is now Phase 2**, downstream of a validated discovery, and must trace to an objective and
  key result before it can be approved.
- **`checkpoint` closes the loop**: writes `OUTCOME.md` recording whether the predicted key result
  actually moved, including the honest "never instrumented".
- **`retro` scores outcomes**, not just process - a zero kill rate or persistent "shipped, never
  measured" is treated as evidence the team has drifted back into a feature factory.
- `new-project` now leads with the product vision, a named target persona, and an optional
  timeboxed Business Model Canvas.
- SessionStart hook updated for the new pipeline and now injects the outcome-over-output rule.

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
