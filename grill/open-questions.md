# Open Questions

Living registry of gaps found while reviewing requirements. Only `Open` and `Answered` entries belong here — once an OQ is `Closed`, its full record moves to [`traceability.md`](./traceability.md) and the entry is removed from this file. See [`README.md`](./README.md) for the full lifecycle.

### OQ-001 — Should `sources.md` become a machine-generated lock file?

**State:** Open
**Category:** Non-blocking
**Applies to:** cross-cutting (`inputs/sources.md`)

**Question:** `inputs/sources.md` is currently a hand-maintained registry. A stricter alternative would be a machine-generated lock file (hash-based, auto-detecting changed/new/removed inputs on each derivation) — but this repo has no derivation tooling to compute or enforce that today. Should `sources.md` stay hand-maintained indefinitely, or is a lock file planned for later, once such tooling exists?

<!--
### OQ-<ID> — <short title>
**State:** Open | Answered | Closed
**Category:** Blocking | Non-blocking
**Applies to:** REQ-<ID> | NFR-<ID> | cross-cutting

**Question:** ...

**Resolved by:** HD-<ID> (see requirements/requirements.md)
-->
