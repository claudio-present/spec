# Contracts

A contract is a formal, machine-checkable definition of an interface —
independent of whatever implements or calls it. Right now every interface
in here happens to be an API (OpenAPI for REST, `.proto` for gRPC, an SDL
for GraphQL, AsyncAPI for events), but this folder isn't scoped to APIs
specifically — a module or library-level contract belongs here too, under
the same owned/consumed split, once one exists.

## Why SDD needs this as its own artifact

A prose spec can say "the service returns the user's order history." It
can't say whether `orders` is a top-level array or nested under `data`,
whether `total` is a number or a string, or what happens on an empty result.
Those details are exactly what break integrations, and prose specs
routinely leave them ambiguous or let them drift from the code silently. A
contract closes that gap: it's specific enough to generate client/server
code from, mock, and check automatically — a spec never is.

## Two folders, two different jobs

- **`owned/`** — interfaces this spec owns. The contract is authoritative:
  written before (or alongside) implementation, and the implementation is
  what has to conform. See [owned/README.md](owned/README.md) for why that
  ordering matters.
- **`consumed/`** — interfaces this spec depends on but doesn't define
  (a third-party API, another team's service, another module this spec
  doesn't control). This spec doesn't get to define these, but it should
  still pin the subset it depends on. See [consumed/README.md](consumed/README.md)
  for why that's worth doing even when you don't own the interface.

Each subfolder has its own README explaining what belongs there and why.
