# Consumed contracts

Interfaces this spec depends on but doesn't own: a third-party API,
another team's service, or another module/package this spec doesn't
control.

## What goes here

Not the dependency's full surface — just the subset this spec actually
relies on, pinned: which endpoints/operations, which schema version, and
the integration assumptions that aren't obvious from its docs (auth flow
used, rate limits assumed, retry/error handling expected). One file or
folder per dependency.

## Why bother, given we don't control it

Because we don't control it, a silent change on the other side has no
other safety net. Pinning what we depend on turns that into something
checkable in CI instead of something that surfaces as a production
incident.

**Source:** [Consumer-Driven Contracts](https://martinfowler.com/articles/consumerDrivenContracts.html)
(Martin Fowler) — the pattern this folder implements.
