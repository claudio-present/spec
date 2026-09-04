# Requirements — Index

Entry point of the spec. Does not contain requirements themselves — those live in `functional/` (behaviors per feature) and `non-functional/` (quality attributes). This file only aggregates the product overview and the human decisions already made.

## Functional

<!-- One link per file in functional/, with a one-line summary -->
- [ ] [`functional/<feature>.md`](./functional/<feature>.md) — <short summary>

## Non-functional

<!-- One link per file in non-functional/, with a one-line summary -->
- [ ] [`non-functional/<attribute>.md`](./non-functional/<attribute>.md) — <short summary>

## Human Decisions

Decisions made about gaps found in a requirement. What's recorded here is the decision itself (the "why"), not the acceptance criterion — that's integrated directly into the `REQ-<ID>`/`NFR-<ID>` it belongs to, in `functional/`/`non-functional/`, so as not to violate the rule that Acceptance Criteria never lives in a separate artifact. Each `HD-<ID>` is the output of the Grill phase (see `grill/`) — here there's only the decision, summarized.

<!--
### HD-<ID> — <short title>
**Applies to:** REQ-<ID> | NFR-<ID> | cross-cutting
**Source:** OQ-<ID> (see grill/open-questions.md or grill/traceability.md once Closed)

**Decision:** ...
(the resulting Acceptance Criteria is already in functional/<feature>.md or non-functional/<attribute>.md, inside the REQ-<ID>/NFR-<ID> above)
-->
