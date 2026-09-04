# Open Questions

Living registry of gaps found while reviewing requirements. Only `Open` and `Answered` entries belong here — once an OQ is `Closed`, its full record moves to [`traceability.md`](./traceability.md) and the entry is removed from this file. See [`README.md`](./README.md) for the full lifecycle.

### OQ-001 — Should `sources.md` become a machine-generated lock file?

**State:** Open
**Category:** Non-blocking
**Applies to:** cross-cutting (`inputs/sources.md`)

**Question:** `inputs/sources.md` is currently a hand-maintained registry. A stricter alternative would be a machine-generated lock file (hash-based, auto-detecting changed/new/removed inputs on each derivation) — but this repo has no derivation tooling to compute or enforce that today. Should `sources.md` stay hand-maintained indefinitely, or is a lock file planned for later, once such tooling exists?

### OQ-002 — Does MoSCoW belong at the input-triage stage?

**State:** Open
**Category:** Non-blocking
**Applies to:** cross-cutting (`inputs/README.md`, `requirements/README.md`)

**Question:** MoSCoW was explicitly dropped from `requirements/` (RFC 2119 already carries binding strength there; prioritization was judged to belong to upstream backlog refinement). A later draft of `inputs/README.md` reintroduces MoSCoW as metadata tagged on a user story *before* it's decomposed into a `REQ`/`NFR` — i.e. at triage time, not inside the requirement itself. Is that placement consistent with the earlier decision (MoSCoW never touches `requirements/`, but may still tag a raw input in `inputs/`), or should MoSCoW stay out of this repo's spec pipeline entirely?

<!--
### OQ-<ID> — <short title>
**State:** Open | Answered | Closed
**Category:** Blocking | Non-blocking
**Applies to:** REQ-<ID> | NFR-<ID> | cross-cutting

**Question:** ...

**Resolved by:** HD-<ID> (see requirements/requirements.md)
-->
