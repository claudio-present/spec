# Versioning (Change Log)

One file per behavior change to something already `Baselined` — an
implemented, shipped requirement, contract, or system-context file. Not
where behavior is first authored (that's `requirements/`, `contracts/`,
`system-context/`); this is the permanent, immutable record of every time
that behavior later changed.

```
versioning/
├── README.md
├── _TEMPLATE.md          # copy into a new versioning/<NNN>-<slug>.md
└── <NNN>-<slug>.md
```

**Deliberately not named `specs/`.** `system-context/README.md` already
promises a future `specs/<NNN>-<name>/system-context.md` — a per-feature
folder for whoever builds that feature. This is still a draft, and keeping
the versioning/change-log concern in its own top-level folder, under its
own numbering, avoids the two colliding while both are still being figured
out independently. If `specs/` gets built out later and this turns out to
belong inside it, that's a follow-up move, not something to guess now.

## The rule: don't edit a `Baselined` requirement, supersede it

`requirements/functional/<feature>.md` (or `non-functional/`,
`contracts/owned/`) stays mutable while it's still `Draft` — edit it
directly, no ceremony, the same as today.

Once a `REQ`/`NFR`/contract has shipped and its `status` front-matter field
is `Baselined` — the requirements-engineering term for a specification
that's been formally agreed and now changes only through a controlled
process, not by editing it back out — that stops. A behavior change to it
now goes through two steps, tracked by the record's own `status`:

1. **`Proposed`** — write `versioning/<NNN>-<slug>.md`: quote the old
   wording verbatim, state the new wording as an explicit override, and
   point at the `HD-<ID>` that decided it (see [`_TEMPLATE.md`](_TEMPLATE.md)).
   The decision is already made at this point (that's what `HD-<ID>` means)
   — this step is only about writing it down before touching anything.
2. **`Accepted`** — edit the living file: apply the new wording, bump its
   `version`, and add this record's path to its `ancestors` list. Then
   come back to the record, fill in its `Applied` section, and flip its
   `status` to `Accepted`.

The living file is still what an agent reads to generate code — it never
has to resolve a version chain to find current truth. `versioning/<NNN>-<slug>.md`
is only for "what did this used to say, and why did it change" — read
rarely, by a human or an agent doing archaeology, not on every generation
pass.

**Only from `Accepted` onward is the record never edited again.** While
`Proposed`, it's still a draft — expected to be touched exactly once more,
to accept it. If an accepted decision gets reversed later, that's a new
numbered file with its own record, `ancestors` pointing back to this one
— not an edit to this one.

## Front matter (every file under `requirements/`, `contracts/owned/`, and every `versioning/<NNN>-<slug>.md`)

```yaml
---
spec_id: REQ-043-sort-order          # or NFR-*, or a contract's own id
title: Sort order for offline processes
status: Baselined                    # Draft | Baselined
version: 1.1.0
external_ref:                        # optional: your tracker's id, if any — no tracker assumed
ancestors:                           # only on a file that has changed since it was baselined
  - versioning/043-change-sort-order.md
overrides:                           # which prior AC(s) this version replaces, and how
  - REQ-012.AC-003 (ascending sort → descending)
applied_decisions:                   # HD-<ID> entries this version reflects, from requirements.md
  - HD-012
---
```

`version` follows [SemVer](https://semver.org): a wording/clarification
fix that changes no behavior is a patch; a new, additive `REQ`/`AC` is
minor; changing or removing behavior that something already depends on is
major. `external_ref` is deliberately generic — this repo makes no
assumption about which issue tracker (or whether one) sits behind it, per
the Technological Independence principle already stated in
[`requirements/README.md`](../requirements/README.md).

## Why an immutable record, when Git already keeps every version

Git already never loses a version — that's not the gap. The gap is that
nothing about a past decision is *addressable by name*. "Why does REQ-012
sort descending now, and what did it say before" means `git log -p`
archaeology across possibly unrelated commits; a reviewer or an agent has
no single file to open. Naming the record (`versioning/043-change-sort-order.md`)
and requiring the old wording, the new wording, and the decision to live
together in it turns that search into reading one file — the same
argument Michael Nygard makes for keeping Architecture Decision Records
instead of relying on commit history alone.

It also keeps the *living* file small: an agent generating code from
`functional/checkout.md` only ever reads the current `REQ`, never a chain
of past ones — the same "don't make the agent re-read what it doesn't
need" reasoning already applied to `open-questions.md`/`traceability.md`
and `requirements.md`/`traceability.md` elsewhere in this repo.

## Commit convention

A commit that changes a `Baselined` requirement's behavior should
reference the record that authorized it, using a trailer so it stays
machine-parseable:

```
fix(checkout): sort offline queue descending

Spec: versioning/043-change-sort-order.md
```

Format follows [Conventional Commits](https://www.conventionalcommits.org/);
`Spec:` is a plain git trailer, not a Conventional Commits footer type, so
tooling that only understands the base spec still parses the rest of the
message correctly.

## Sources

- [Documenting Architecture Decisions](https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions.html)
  (Michael Nygard) — the core pattern this file implements: a decision,
  once accepted, is never edited, only superseded by a new record that
  says so explicitly. `Proposed`/`Accepted` and "never edit an `Accepted`
  record again" both come directly from ADR status vocabulary (also
  [MADR](https://adr.github.io/madr/)).
- [*Software Requirements*, 3rd ed.](https://www.microsoftpressstore.com/store/software-requirements-9780735679665)
  (Karl Wiegers, Joy Beatty; Microsoft Press, ISBN 978-0-7356-7966-5) —
  `status: Baselined` and the "changes only through a controlled process"
  rule are the requirements-baseline / change-control-board practice this
  file implements for a single-repo, agent-driven pipeline instead of a
  CCB meeting.
- [OpenSpec](https://github.com/Fission-AI/OpenSpec) — the delta-only
  change-record shape (one record per change, not a rewrite of the whole
  spec), already the source for `system-context/README.md`'s per-feature
  folders.
- [Semantic Versioning](https://semver.org) — the `version` field's
  meaning.
- [Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
  (Martin Fowler) — the underlying principle that facts, once recorded,
  are appended to, never mutated; `ancestors`/`overrides` is that applied
  to spec files instead of application state.
- [Conventional Commits](https://www.conventionalcommits.org) — the commit
  message convention referenced above.
