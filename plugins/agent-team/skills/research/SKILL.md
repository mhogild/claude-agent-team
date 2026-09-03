---
name: research
description: 'Phase 2 — read-only exploration of codebase and domain that compacts into RESEARCH.md, so the plan is built on facts not guesses. Runs in its own context; delegates heavy search to Explore subagents.'
---

# research

**Runs in its own context.** Spawned as a subagent by the autonomous driver, or invoked directly.
Input: `SPEC.md` + the codebase. Output: `.planning/<feature>/RESEARCH.md`.
Second phase — see `${CLAUDE_PLUGIN_ROOT}/WORKFLOW.md` (or run `/agent-team:team-map`).

## Why this phase is worth its own context

Research is the highest-leverage review point in the pipeline: a bad line of research can cause
*thousands* of bad lines of code downstream. It's also the noisiest work — file searches, greps,
dependency tracing flood a context window with low-signal tokens. So it gets its own context, and
the noise is compacted into one small artifact before planning starts.

## Steps

1. **Delegate the search.** Spawn built-in `Explore` subagents for the grunt work (find the files,
   the patterns, the seams). They run in isolated context and return condensed summaries — the
   noise never touches this context. Specify each with a clear objective, scope, and output format;
   vague instructions cause duplicate work and gaps.
2. **Synthesise, don't transcribe.** RESEARCH.md is a distillation, not a log: affected files and
   why, existing patterns to reuse, the real root cause / data flow, integration risks, and the
   options you see (with trade-offs) — not a paste of everything you read.
3. **Name the integration risks explicitly.** Any auth / payments / third-party-SDK dependency
   that the plan will rest on gets flagged here as needing a real end-to-end spike before build.
4. **Stay read-only.** Research explores; it does not edit production files.

## Exit gate

RESEARCH.md exists, is compact, and its author asserts it — the reviewer checks it, nobody waits
for approval (`ESCALATION.md` §2). Keep this context's utilization modest: when it gets heavy,
you've got enough to write the artifact. Write it, then hand any remaining questions to a
fresh-context subagent and continue — never stall waiting to be restarted.
