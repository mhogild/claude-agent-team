---
name: discovery
description: 'Phase 1 - frame one opportunity, generate solutions, prototype, and retire the four big risks (value, usability, feasibility, viability) before anything is specced or built. Produces DISCOVERY.md ending in validated or killed. Run by the product trio.'
---

# discovery

**Fresh window.** Input: `.planning/STRATEGY.md` + one candidate problem.
Output: `.planning/<opportunity>/DISCOVERY.md`. Owner: `agents/product-manager.md`, with
`agents/product-designer.md` (usability) and `agents/tech-lead.md` (feasibility) — the product trio.

This is the phase the team was missing. Everything downstream of an approved spec was strong; there
was nothing upstream to decide whether the spec deserved to exist. **Discovery's job is to kill bad
ideas cheaply, in days, before delivery spends weeks.**

A discovery that has never killed anything is not discovery. Expect to close a real share of them
as *killed* — that is the phase working, not failing, and it should be recorded with the same care
as a success.

## 1. Framing

Get agreement on the problem before anyone proposes a solution. Write the **opportunity
assessment** — four questions, answered in a few lines each:

1. **What business objective is this?** Name the objective and the key result from `STRATEGY.md`.
   If it maps to none, stop: this is not ready, and building it means the strategy was wrong or the
   idea is someone's pet.
2. **What are the expected key results?** The specific number you expect to move, and by roughly
   how much. Write the guess down *now* — a prediction recorded before the work is the only honest
   way to learn later whether you understood the problem.
3. **What is the customer problem?** From the customer's side, in their words, not as a missing
   feature. "I can't tell which of my tills is short" is a problem. "We need a reconciliation
   report" is a solution wearing a problem's clothes.
4. **Who is the target persona?** One. From `STRATEGY.md`.

For something large enough to change the product's story, write a **customer letter** instead: a
short note from a named future customer describing how their life improved, plus the internal FAQ
of hard questions it raises. Working backward from the happy customer exposes vagueness that a
feature list hides.

Then **name the risks and rank them by what is most likely to kill this**, using the four:

| Risk | Question | Owner |
|---|---|---|
| Value | Will they use it / buy it? | product-manager |
| Usability | Can they figure it out? | product-designer |
| Feasibility | Can we build it with what we have? | tech-lead |
| Business viability | Does it work for the business — cost, legal, brand, support, GTM? | product-manager |

Attack the most-likely-fatal first. Usually that is **value**, and it is usually the one skipped.

## 2. Ideation

Generate **several** candidate solutions to the framed problem before choosing. If exactly one
solution was ever on the table, you did not do ideation, you did rationalisation.

Useful sources: the target persona's actual workarounds today, the support/complaint surface, what
the data says people already do, and deliberately cheap or embarrassing options — the "what if we
just..." ones are frequently right and are usually dismissed too fast.

Record which you rejected and why. That record is what stops the team relitigating them at build.

## 3. Prototyping

`product-designer` builds the **cheapest artifact that can answer the riskiest open question** —
feasibility spike, user prototype, live-data prototype, or Wizard-of-Oz. Fidelity follows risk,
never taste. It is throwaway by construction.

Never let production code be the first artifact of an unvalidated idea. Once code exists, the team
is committed to it and discovery is over whether or not it concluded.

## 4. Testing the four risks

Test only what is actually open — a risk that is genuinely obvious can be marked *accepted* with a
one-line reason. What you may not do is leave one silently unexamined.

- **Value** — hardest and most important. Evidence that someone will change behaviour: they use the
  prototype unprompted, pay, pre-commit, or abandon their workaround. Opinions ("that looks useful")
  are not value evidence and must not be recorded as if they were.
- **Usability** — a real user, a task not a tour, watched not asked. See `agents/product-designer.md`.
- **Feasibility** — `tech-lead` spikes the genuinely unknown part: the integration, the performance
  budget, the thing nobody here has built before. Not the parts already known to work.
- **Business viability** — walk it past the parts of the business it touches: cost to serve, legal
  and compliance, security, support load, brand, pricing/GTM. For a regulated surface this is often
  the real blocker and is discovered embarrassingly late.

## 5. Close it

`DISCOVERY.md` ends with an explicit verdict:

- **Validated** — each of the four risks retired with stated evidence, or explicitly accepted by the
  human in writing with a reason. Proceeds to `/agent-team:spec`.
- **Killed** — say which risk killed it and what the evidence was. This is a success. Record it so
  the idea does not return in three months with no memory.
- **Pivoted** — the problem is real, this solution isn't. Reframe and re-enter at step 1.

**Timebox the whole phase.** Discovery that runs longer than the delivery it protects has become
procrastination. Days, not weeks.

## Exit gate

Opportunity assessment written and traced to an objective · more than one solution considered ·
prototype built at a fidelity justified by risk · all four risks retired-with-evidence or
explicitly-accepted-in-writing · verdict recorded · human approved.

## When to skip this

Be honest rather than ceremonial. A change with no value risk — a bug fix, a compliance obligation,
a refactor, a change the human has directly asked for and will personally use — does not need a
discovery phase. Note in the SPEC that discovery was skipped and why. What you may not do is skip it
on something *speculative* because you are impatient to build; that is the exact moment it pays.
