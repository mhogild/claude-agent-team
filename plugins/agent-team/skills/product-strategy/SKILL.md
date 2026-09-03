---
name: product-strategy
description: 'Level 1.5 - decide what the team will focus on and what result it must produce, before any opportunity is framed. Produces STRATEGY.md with focus, insights, target persona, and business objectives with measurable key results. Run by product-manager, per milestone.'
---

# product-strategy

**Runs in its own context.** Spawned as a subagent by the autonomous driver, or invoked directly.
Input: `PROJECT.md` (vision) + whatever you know about the business.
Output: `.planning/STRATEGY.md`. Owner: `agents/product-manager.md`. Run once per milestone, not
per feature.

This phase exists because a roadmap of features answers *what we will build* and never answers
*what has to become true*. Without it the team is a delivery factory with good tooling.

## The chain

`vision (PROJECT.md) -> strategy (focus + insights) -> business objectives + key results -> framed opportunities (/agent-team:discovery)`

Strategy is how the vision becomes real while meeting the needs of the business as you go. Every
opportunity the team later frames must trace up this chain. If it can't, it's someone's pet
feature.

## Steps

1. **Focus — choose few.** Name at most **two or three** objectives for this milestone. Most
   strategies fail not from bad choices but from the avoidance of choosing: fifty simultaneous
   initiatives and no priority. Write down what you are explicitly **not** pursuing this milestone;
   that list is the strategy, as much as the chosen list is.

2. **Insights — write what you actually know.** What has the data, the customers, the industry or
   your own capabilities told you that most teams in this space don't act on? An insight is a
   claim about reality that could turn out false, not a restatement of the goal. If you have no
   insights, say so — an honest "we are guessing" is strategy; a confident fabrication is not.

3. **Target persona — one at a time.** Name the single user you are optimising this milestone for,
   concretely enough to disagree with: their job, their day, what they currently do instead, and
   what makes them abandon a tool. Note the users you are knowingly *not* serving well yet.
   Serving everyone at once is how a product becomes nobody's favourite.

4. **Obsess over the customer, not the competitor.** Competitor behaviour is an input, never a
   goal. "Because a competitor has it" is not a business objective and must not appear as one.

5. **Business objectives and key results.** For each objective:
   - The **objective is qualitative** and states what must be *accomplished* — never what the team
     will *do*. "Cashiers trust end-of-day totals" is an objective. "Ship the reconciliation
     screen" is a task wearing an objective's clothes.
   - The **key results are quantitative** and measure a **business result**, not a deliverable.
     "Reduce unexplained till variance by 30% by end of milestone" is a key result. "Reconciliation
     screen shipped" is not — it is trivially easy to ship a deliverable and not move the problem.
   - State how each key result will actually be **measured**, and whether you can measure it today.
     A key result with no instrument is a wish; either add the instrument to the milestone or admit
     the objective is unmeasurable and say so in writing.

6. **Business alignment.** State in one line, per objective, how it serves the business — revenue,
   cost, risk, retention, or compliance. If you can't write that line, the objective doesn't
   belong in this milestone — cut it, and say in STRATEGY.md that you cut it and why.

7. **Assert it and proceed.** STRATEGY.md is the team's to set — see `ESCALATION.md` §2. It sets
   what the whole milestone is judged against, so state the assertion in the document, name the
   reasoning behind the focus, and go to discovery. Objectives are revisable by a later
   milestone; a strategy waiting for a signature is a milestone not started.

## An honest warning about OKRs

Cagan spent years advocating this technique and then largely stopped recommending it, because in
most organisations it becomes ceremony that consumes effort and yields nothing. It is specifically
a cultural mismatch for teams that are handed features to build rather than problems to solve.

So: use this lightly. **One objective with one or two real key results, honestly measured, beats a
scorecard nobody revisits.** If you are a solo builder or a very small team, the useful residue of
this whole phase is four lines — the problem, who has it, what result would prove it solved, and
what you're not doing. Write those four and move on. If you find yourself maintaining an OKR
document that never changes a decision, delete it; that is the failure mode, not a lapse in rigour.

## Business Model Canvas

Useful once, early, to see whether the thing hangs together commercially — segments, value prop,
channels, revenue, costs. **Timebox it to a single sitting** and keep it in `PROJECT.md`. It is a
thinking aid, not an artifact to maintain; a canvas being lovingly revised is a team avoiding
customer contact.

## Exit gate

Asserted by product-manager, verified by the reviewer — not waited on (`ESCALATION.md` §2). Focus
named (and non-goals named) · target persona defined · each objective qualitative · each key
result quantitative, business-level, and measurable with a stated instrument · business alignment
stated per objective.
