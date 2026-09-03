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

**Feature (7 sections):** Scope & Boundaries · Component System Context
(C4 L3) · Interface & Data Contracts (C4 L4) · Dynamic Behavior · Error &
Reason Code Mapping · Mechanical Controls & NFRs (feature-specific only) ·
QA Isolation Rules (restated for this feature's own tests).

## Why this format

- **Two levels, not one.** Mixing global rules with feature detail is
  "context pollution" — an agent reads what's irrelevant, drifts, and
  invents architecture to fill the gaps.
- **Diagrams over prose at component level.** "Module A talks to B" leaves
  the path, data shape and direction to invent; a C4 diagram doesn't.
- **Contracts before code.** Every field traces to a requirement, or is
  flagged as not one — so nothing invented during derivation looks sourced.
- **Explicit state machines.** Prose hides forgotten states and simultaneous
  transitions; a diagram forces full enumeration.
- **Deterministic error mapping.** Otherwise exception handling is left to
  whoever implements it, inconsistently across features.
- **NFRs need a metric and a target.** "Should be fast" isn't verifiable —
  a CI gate needs a number to check against.
- **QA isolation as a rule, not a reminder.** Whoever is graded shouldn't be
  able to alter the grading.

## Sources

Verified independently (web search, not this conversation's own documents) —
each one names an existing, real practice this template borrows from, never
taken on faith:

- Components are drawn as a diagram, not described in prose, the same way
  the [C4 model](https://c4model.com) (Simon Brown) separates a Component
  level from a Code level — sections 2 and 3 of `feature-level.md` are
  named directly after C4 Levels 3 and 4.
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
- NFRs need a metric and a target rather than a description, following Tom
  Gilb's Planguage, where every quality attribute carries a
  [Scale, a Meter and a Target](https://www.methodsandtools.com/archive/archive.php?id=91).
- Dynamic behavior is modeled as an explicit state machine, the same
  notation David Harel introduced as
  [statecharts](https://en.wikipedia.org/wiki/Harel_statechart) in 1987 —
  the direct ancestor of the Mermaid `stateDiagram-v2` used in the examples
  here.
- EARS (Alistair Mavin, Rolls-Royce; [official guide](https://alistairmavin.com/ears/))
  is a real, widely adopted way to write unambiguous requirements, but it
  hasn't been worked into either file yet — the natural next step if
  "Error & Reason Code Mapping" or "Data Contracts" need to become
  mechanically checkable.

What isn't here: project identity, folder pattern, composition mechanism,
global invariants, and the shape of "Error & Reason Code Mapping" follow no
external source found and verified — general practice, stated as such
rather than attributed on a guess.
