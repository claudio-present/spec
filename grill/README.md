# The Grill Phase

Before a gap in a requirement becomes code, it goes through the Grill: an adversarial review where the AI and the analyst challenge the spec to find omissions, ambiguities, or unmapped behavior. The goal is to stop development based on guesses (*vibe coding*) before it starts.

> Source: [github/spec-kit](https://github.com/github/spec-kit)

---

## Files in `grill/`

```
grill/
├── README.md              # this file: lifecycle + rules
├── open-questions.md      # living registry — only active OQs (Open / Answered)
└── traceability.md        # permanent archive — Closed OQs, with the full discussion
```

- **`open-questions.md`** holds only what's still active. It's meant to stay small — an agent scanning it should immediately see what's blocking what.
- **`traceability.md`** is where everything about a question goes once it's Closed — not just the decision: context, meeting notes, rejected alternatives and why, research gathered along the way. Kept apart from the active file to protect the generation agent's context window from **context rot** (the decline in response quality as the context window fills up).

> Source: [open-gsd/gsd-core](https://github.com/open-gsd/gsd-core)

---

## Lifecycle: OQ → HD → Traceability

```
  [DETECTION]              [HUMAN DECISION]           [ARCHIVE]

  ┌────────┐              ┌──────────────┐          ┌─────────────────┐
  │ OQ-ID  │ ───────────> │    HD-ID     │ ───────> │  traceability.md │
  └────────┘              └──────────────┘          └─────────────────┘
  State: Open              State: Answered            State: Closed
```

### 1. OQ (Open Question) — detection

When a gap is found in a requirement (e.g. *"what happens if the network drops mid-write?"*), it's recorded as an entry in `open-questions.md`:

- **State:** `Open`
- **Category:** `Blocking` or `Non-blocking` — see below
- **Applies to:** the `REQ-<ID>` / `NFR-<ID>` it affects, or `cross-cutting`

**Every gap always gets asked to a human — no exceptions.** `Category` only decides whether the pipeline *waits* for the answer; it never authorizes the AI to resolve the doubt itself or skip the question. Whatever the doubt, `Blocking` or `Non-blocking`, it goes through step 2 and reaches `Answered`; there is no path from `Open` straight to `Closed`, and no path that bypasses a human decision-maker.

There is no separate pipeline-state file (no `STATE.md`). The gate is a rule derived directly from this registry: **a `REQ`/`NFR` is gapped if it has an `Open` OQ with `Category: Blocking` applied to it.** Nothing to keep in sync — the registry is the single source of truth.

### 2. HD (Human Decision) — resolution

A decision-maker (PO, architect, stakeholder) answers the question. The decision is:

1. Synthesized into an EARS-worded Acceptance Criterion (with the right RFC 2119 keyword) and injected directly into the `REQ-<ID>`/`NFR-<ID>` it belongs to, in `functional/<feature>.md` or `non-functional/<attribute>.md`.
2. Catalogued as `HD-<ID>` in `requirements/requirements.md`'s Human Decisions, with **Source** pointing back to the OQ.
3. Reflected in the OQ entry: `State` moves to `Answered`.

> Source: [Business Requirement to Functional Spec — Analyst Engineering](https://www.analystengineering.com/articles/business-requirement-to-functional-spec)

### 3. Traceability — archive

Once the decision is fully documented, the OQ is marked `Closed`: everything about it — context, meeting notes, rejected alternatives, research, the decision — moves permanently into `traceability.md`, and the entry is removed from `open-questions.md`. What remains is a clean, bidirectional trail: `AC` in the requirement file → `HD-<ID>` in `requirements.md` → full log in `traceability.md`.

---

## State and Category reference

| State | Meaning |
|---|---|
| `Open` | Unanswered. No decision made yet. |
| `Answered` | Decision made; the Acceptance Criterion has been (or is being) injected into the requirement file and the `HD-<ID>` catalogued. |
| `Closed` | Fully archived — the entry has moved from `open-questions.md` to `traceability.md`. |

| Category | Pipeline impact | Still needs a human answer? |
|---|---|---|
| `Blocking` | Halts code/test generation for the affected `REQ`/`NFR` until the OQ is `Answered`. | Yes |
| `Non-blocking` | Advisory. Research or prototyping on the affected area may continue in parallel. | Yes |

`Category` never exempts an OQ from being answered — it only decides whether the pipeline sits idle while waiting.
