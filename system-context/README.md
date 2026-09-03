# System Context

## What it is

System Context delimits, before a line of code is written, what already
exists around it — which components exist, who talks to whom, what crosses
each edge, what belongs to some other part of the system. Not solution
architecture, not code: the boundary contract that precedes both.

It exists at **two levels**, each its own file:

| File | Level | Answers |
|---|---|---|
| [`project-level.md`](project-level.md) | System (1 per project) | What is always true, for any feature? |
| [`feature-level.md`](feature-level.md) | Feature (1 per feature) | What is specific here, given what is always true? |

One `project-level.md` per project (commonly renamed `AGENTS.md`,
`GROUNDING.md` or `constitution.md`). Each feature gets its own copy of
`feature-level.md`, renamed `system-context.md`, at
`specs/<NNN>-<name>/system-context.md`.

## What it must contain

**Project (8 sections):** Identity · Folder/Module Pattern · Composition
Mechanism · Sanctioned Communication Channels (no silent third one) · State
Management & Routing · Global Invariants (identity, persistence, security) ·
Mechanical Controls · QA Isolation Rule.

**Feature (4 sections):** Scope & Boundaries · Component System Context
(C4 L3) · Dynamic Behavior · QA Isolation Rules (restated for this feature's
own tests).

Capped at C4 Level 3 on purpose. Data contracts, endpoints, DTOs and error
codes (C4 Level 4) belong to this feature's own **API contracts +
DataModels** document instead — a different topic in the same pipeline, not
a deeper section of this one.

## Why this format

- **Two levels, not one.** Mixing global rules with feature detail is
  "context pollution" — an agent reads what's irrelevant, drifts, and
  invents architecture to fill the gaps.
- **Diagrams over prose at component level.** "Module A talks to B" leaves
  the path, data shape and direction to invent; a C4 diagram doesn't.
- **Explicit state machines.** Prose hides forgotten states and simultaneous
  transitions; a diagram forces full enumeration.
- **QA isolation as a rule, not a reminder.** Whoever is graded shouldn't be
  able to alter the grading.
- **Capped at C4 Level 3.** Data contracts, error codes and DTOs are a
  different kind of thing than a component diagram — mixing them in is how
  a "system context" document quietly grows into doing Requirements' job
  (non-functional requirements), the Grill Phase's job (parking an open
  question here instead of raising it), and the API contracts document's
  job (data shapes), all at once.

## Sources

Verified independently (web search, not this conversation's own documents) —
each one names an existing, real practice this template borrows from, never
taken on faith:

- Components are drawn as a diagram, not described in prose, the same way
  the [C4 model](https://c4model.com) (Simon Brown) treats Component as its
  own level, distinct from Code — section 2 of `feature-level.md` is named
  directly after C4 Level 3, and stops there on purpose.
- `project-level.md` works as one persistent document every feature defers
  to and never contradicts, the same role [GitHub Spec Kit](https://github.com/github/spec-kit)
  gives its `constitution.md`: every other phase there (spec, plan, tasks)
  checks against it, the way every `feature-level.md` here checks against
  `project-level.md`.
- Each feature gets its own folder with its own context file, similar to how
  [OpenSpec](https://github.com/Fission-AI/OpenSpec) gives each capability
  its own folder with a `spec.md` plus an optional `design.md` — this
  template just calls the unit "feature" instead of "capability".
- Splitting system from feature at all is meant to stop an agent from
  loading context irrelevant to what it's implementing, the same problem
  Addy Osmani names the "curse of instructions" in
  ["How to write a good spec for AI agents"](https://addyo.substack.com/p/how-to-write-a-good-spec-for-ai-agents).
- Scope & Boundaries asks for the relationship to what's excluded, not just
  what's included, as suggested by Martin Fowler's
  ["Bounded Context"](https://martinfowler.com/bliki/BoundedContext.html) —
  a boundary is only useful once it also says what's on the other side of it.
- Dynamic behavior is modeled as an explicit state machine, the same
  notation David Harel introduced as
  [statecharts](https://en.wikipedia.org/wiki/Harel_statechart) in 1987 —
  the direct ancestor of the Mermaid `stateDiagram-v2` used in the examples
  here.

What isn't here: project identity, folder pattern, composition mechanism,
and global invariants follow no external source found and verified —
general practice, stated as such rather than attributed on a guess.
