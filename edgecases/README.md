# Edge cases

Numbered conditions (`EC-1`, `EC-2`, ...) outside the happy path — two
different kinds, both in scope:

- **Boundary conditions** — valid, expected inputs or states that sit at an
  edge and need specific, correct (non-error) behavior defined: an empty
  list, a single-item list, a value at the max allowed limit, two requests
  arriving at once.
- **Failure modes** — something actually breaks: a dependency (see
  [../contracts/consumed/](../contracts/consumed/)) is slow, down, or
  returns garbage; bad input that must be rejected; invalid internal state.

A failure mode is one kind of edge case, not the whole category — a
boundary condition can have perfectly well-defined behavior with nothing
failing at all, and still needs to be specified just as deliberately.

## Why this is separate from `spec.md`

`spec.md` describes the happy path. Splitting edge cases into their own
file forces "what happens at this boundary, or when this goes wrong?" to
be asked deliberately for every dependency and risky assumption, instead
of being covered only when someone happens to think of it while writing
prose.

## What goes here

One entry per edge case, numbered so acceptance criteria and tests can
reference it by id instead of restating it:

- **EC-N: <condition>** — expected behavior, and whether it's a boundary
  condition or a failure mode. If it relates to a contract or dependency,
  link into `contracts/owned/` or `contracts/consumed/`; if not (bad input,
  invalid internal state, a boundary value, timing/ordering issues), it
  stands on its own.

**Source:** *The Art of Software Testing*, 3rd ed. (Glenford Myers, Tom
Badgett, Corey Sandler; Wiley, ISBN 978-1-118-03196-4) — boundary value
analysis and equivalence partitioning (including invalid/error partitions)
are the origin of both halves of this folder's definition.
