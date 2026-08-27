---
name: diagrams
description: 'Produce and maintain the project''s visual docs — architecture, ERD, data-flow, and key sequence diagrams — as Mermaid so they render on GitHub and diff in git. Run by solution-lead, kept true at checkpoint.'
---

# diagrams

Owner: `agents/solution-lead.md`. Runs as part of `/agent-team:checkpoint`, or on demand when the shape of
the system changes materially. Output lives in the durable-docs set (see below), not in throwaway
chat.

## Format: Mermaid, always

Write diagrams as **Mermaid** fenced code blocks inside markdown. Reasons: GitHub renders them
inline, they diff in git (no binary blobs), they're token-cheap for an agent to read and edit, and
they can't drift into a separate un-versioned tool. Do **not** commit exported PNG/SVG as the
source of truth; if a raster export is ever needed, generate it from the Mermaid, don't hand-edit.

## The visual docs this project keeps

- **`docs/ARCHITECTURE.md`** — a component/system diagram (Mermaid `flowchart`) showing the major
  pieces (client/UI surfaces, API/services, data stores, third-party integration boundaries) and how
  they talk. Include a short data-flow view for the product's critical path end to end.
- **`docs/ERD.md`** — the data model as a Mermaid `erDiagram`: entities, key attributes,
  relationships and cardinality (products, variants, inventory, orders, line items, payments,
  customers, users/roles, stores/registers). This is the one you asked for by name.
- **`ROADMAP.md`** (repo root) — phase breakdown with the **"▶ you are here"** marker. A Mermaid
  `gantt` or a simple phase list, whichever stays honest with less effort.
- **Key `sequence` diagrams** — only for flows worth the ink: checkout/payment authorization,
  refund/void, auth/login. Add one when a flow is non-obvious across components; don't diagram
  every CRUD path.

## Rules

- **Derive from truth, not imagination.** Build diagrams from the SPEC, the PLAN, and the actual
  code/schema — not a plausible-looking guess. A confidently-wrong ERD is worse than none.
- **A stale diagram is a bug.** At every `/agent-team:checkpoint`, confirm each diagram still matches what was
  built (cross-check the schema and the wiring against `git`), or update it. The
  "newcomer-resumes-from-disk" standard applies to the pictures too.
- **One source of truth per concern.** One architecture diagram, one ERD. If two diagrams disagree,
  that's the failure the checkpoint exists to catch.
- **Keep it legible.** Split an over-dense diagram into a high-level view plus a focused sub-view
  rather than one unreadable wall of nodes.
