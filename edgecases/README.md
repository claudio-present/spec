# Edge cases

Numbered conditions (`EC-1`, `EC-2`, ...) outside the happy path — two
different kinds, both in scope:

- **Boundary conditions** — valid, expected inputs or states that sit at an
  edge and need specific, correct (non-error) behavior defined: an empty
  list, a single-item list, a value at the max allowed limit, two requests
  arriving at once.
- **Failure modes** — something actually breaks in a way that isn't already
  obvious from a contract: a dependency (see
  [../contracts/consumed/](../contracts/consumed/)) is slow, down, or
  returns garbage; invalid internal state; a timing or ordering problem.

A failure mode is one kind of edge case, not the whole category — a
boundary condition can have perfectly well-defined behavior with nothing
failing at all, and still needs to be specified just as deliberately.

Routine, expected error handling isn't an edge case — a required field
that's missing, a malformed request body, an unauthenticated call. If a
contract already documents the response for it (see
[../contracts/owned/](../contracts/owned/)), it doesn't need restating
here. This file is for the conditions that need deliberate thought
*because* they aren't already spelled out elsewhere.

## What goes here

One entry per edge case, numbered so acceptance criteria and tests can
reference it by id instead of restating it:

- **EC-N: <condition>** — expected behavior, and whether it's a boundary
  condition or a failure mode. If it relates to a contract or dependency,
  link into `contracts/owned/` or `contracts/consumed/`; if not (invalid
  internal state, a boundary value, timing/ordering issues), it stands on
  its own.

**Source:** *The Art of Software Testing*, 3rd ed. (Glenford Myers, Tom
Badgett, Corey Sandler; Wiley, ISBN 978-1-118-03196-4) — boundary value
analysis and equivalence partitioning (including invalid/error partitions)
are the origin of both halves of this folder's definition.
