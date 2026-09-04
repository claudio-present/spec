# requirements.md

# Writing Rules

## What makes a good requirement

A good requirement is one behavior, stated so plainly you could hand it to someone else to test.

- **One statement, one `SHALL`/`MUST`.** If a requirement has three "and also" clauses, it's really three requirements. Split them.
- **Observable.** Someone outside the code should be able to tell whether it holds. "The system SHALL show an error banner when the upload exceeds 10 MB" is observable. "The system SHALL handle large uploads gracefully" is not.
- **The right strength.** Stated with the correct RFC 2119 keyword — see below.

The test for a requirement: *could a tester who's never seen the code tell whether it passed?* If not, it needs sharpening.

> Source: [OpenSpec — Writing Specs](https://github.com/Fission-AI/OpenSpec/blob/main/docs/writing-specs.md)

---

## Technological Independence

As advocated by Spec Kit, Spec-Driven Development (SDD) is founded on the premise that the engineering process should not be tied to specific stacks, languages, or frameworks.

---

## The 2 Building Blocks of a Requirement

RFC 2119 and EARS each answer a different question about a requirement — how *binding* it is, and how *precisely* it's stated. Together they leave nothing about a requirement ambiguous.

### RFC 2119 Keywords

**What it is:** three keywords — `MUST`/`SHALL`, `SHOULD`, `MAY` — that OpenSpec uses to mark exactly how binding a requirement is.

| Keyword | Meaning |
|---------|---------|
| `MUST` / `SHALL` | A hard requirement. Non-negotiable. |
| `SHOULD` | A strong recommendation, with room for a justified exception. |
| `MAY` | Genuinely optional. |

Reach for `MUST`/`SHALL` by default. Use `SHOULD` only when you truly mean "unless there's a good reason not to."

**What it's for:** without a marked keyword, "the system should validate the email" could mean either an order or a suggestion, and every reader has to guess which. RFC 2119 removes that guess by naming the binding level in the sentence itself.

**Why it's indispensable:** a requirement is only useful if everyone reading it — including an AI generating code from it — agrees on how binding it is. Get this wrong and a "nice to have" gets built as a blocker, or a hard requirement gets silently skipped as optional.

> Source: [OpenSpec — Writing Specs](https://github.com/Fission-AI/OpenSpec/blob/main/docs/writing-specs.md)

### EARS Syntax as a Logical Predicate Engine

**What it is:** EARS (Easy Approach to Requirements Syntax) is a small set of sentence templates — one keyword per condition (`WHILE`, `WHEN`, `WHERE`, `IF...THEN`) — that a requirement must be written in.

**What it's for:** free-form prose lets you write a requirement without ever stating *when* it applies. "The app should handle errors well" never says what triggers it or what "well" means. EARS forces that condition and that response into the sentence itself.

**Why it's indispensable:** each EARS sentence maps directly onto one test case (trigger → expected result) and, for an AI generating code, onto one `if`/`while` branch. Skip EARS and a requirement can still "sound" complete while leaving out the exact condition a developer or agent needs to implement it correctly.

> Source: [EARS — Alistair Mavin](https://alistairmavin.com/ears/)

#### The 5 Structural Patterns

1. **Ubiquitous** — Always active (no dedicated keyword).

   ```
   The <system name> shall <system response>
   ```

   *Example:* The mobile phone shall have a mass of less than XX grams.

2. **State-driven** — Active while the state holds true, mapped by the `WHILE` keyword.

   ```
   While <precondition(s)>, the <system name> shall <system response>
   ```

   *Example:* While there is no card in the ATM, the ATM shall display "insert card to begin".

3. **Event-driven** — Response to an instantaneous trigger, mapped by `WHEN`.

   ```
   When <trigger>, the <system name> shall <system response>
   ```

   *Example:* When "mute" is selected, the laptop shall suppress all audio output.

4. **Optional Feature** — Applied only if a specific feature exists in the system, mapped by `WHERE`.

   ```
   Where <feature is included>, the <system name> shall <system response>
   ```

   *Example:* Where the car has a sunroof, the car shall have a sunroof control panel on the driver door.

5. **Unwanted Behavior** — Handling of failures and exceptions, mapped by `IF... THEN`.

   ```
   If <trigger>, then the <system name> shall <system response>
   ```

   *Example:* If an invalid credit card number is entered, then the website shall display "please re-enter credit card details".

---

## Closing a Gap: From Open Question to Acceptance Criteria

Open Questions and Acceptance Criteria are two ends of the same movement: an incomplete requirement becoming a verifiably complete one.

### Open Questions

**What it is:** a question about a requirement that hasn't been decided yet, tracked as an `OQ-<ID>` with a `State` (`Open`/`Answered`/`Closed`) and a `Category` (`Blocking`/`Non-blocking`) in `grill/open-questions.md` — see [`grill/README.md`](../grill/README.md) for the full lifecycle. A `Blocking` OQ still in `Open` state signals the requirement is "gapped" (incomplete).

> Source: [github/spec-kit](https://github.com/github/spec-kit)

**What it's for:** it stops a half-decided requirement from silently becoming code. The gap has to be answered explicitly before the requirement moves forward — nothing gets generated from a guess.

**Why it's indispensable:** once answered, the decision is recorded as a Human Decision in `requirements.md`, and the human clarification is translated into a clean, EARS-written Acceptance Criterion injected directly into the `REQ`/`NFR` it belongs to, in `functional/`/`non-functional/` (see below) — never left floating in `requirements.md` itself.

> Source: [Business Requirement to Functional Spec — Analyst Engineering](https://www.analystengineering.com/articles/business-requirement-to-functional-spec)

The question itself, and the detailed record of logical discussions and product meeting notes behind its answer, belong to the Grill phase — not to `requirements/`. It's tracked in `grill/open-questions.md` while active, and archived to `grill/traceability.md` once the OQ is `Closed`, kept apart to protect the generation agent's active context from context rot. Every `HD-<ID>` in `requirements.md` is the output of that phase, and its **Source** field points back to the originating `OQ-<ID>`.

> Source: [open-gsd/gsd-core](https://github.com/open-gsd/gsd-core)

### Acceptance Criteria

**What it is:** the checklist that says exactly when a `REQ`/`NFR` counts as done.

**What it's for:** it turns a resolved requirement into something testable — the same "could a tester who's never seen the code tell whether it passed?" bar from the top of this file, applied per requirement.

**Why it's indispensable:** in the Spec-as-Source paradigm (and in the practices of leading tools such as Amazon Kiro and OpenSpec), Acceptance Criteria must **NOT** go into a separate artifact. They live inside the same `functional/<feature>.md` or `non-functional/<attribute>.md` file as the `REQ`/`NFR` they belong to — never in `requirements.md`, never in a standalone checklist — so the behavior and its definition of "done" can never drift apart.

---

# Structure of `requirements/`

This section explains *how* the `requirements/` folder is organized and *why*. The rules above explain how to write a good requirement (EARS, RFC 2119); this section explains where everything actually lives and how it flows between files.

## File tree

```
requirements/
├── README.md                    # this file: writing rules + folder structure
├── requirements.md              # product index + Human Decisions
├── functional/
│   ├── _TEMPLATE.md             # template for a new capability/feature
│   └── <feature>.md             # one file per capability (REQ-*)
└── non-functional/
    ├── _TEMPLATE.md             # template for a new quality attribute
    └── <attribute>.md           # one file per attribute (NFR-*), e.g. performance.md, security.md
```

`traceability.md` no longer lives here — it moved to [`grill/traceability.md`](../grill/traceability.md), alongside `grill/open-questions.md`. See [`grill/README.md`](../grill/README.md) for that phase's own structure.

## Role of each file

| File | Contains | Does not contain |
|---|---|---|
| `README.md` | Writing rules (EARS, RFC 2119) + this structure | Concrete requirements |
| `requirements.md` | Index (links to `functional/`/`non-functional/`) and `Human Decisions` (the "why" behind each decision already made) | Requirements, Acceptance Criteria |
| `functional/<feature>.md` | `REQ-<ID>` (behavior, in EARS) + its Acceptance Criteria | Discussion/reasoning behind the decision |
| `non-functional/<attribute>.md` | `NFR-<ID>` (constraint/quality, in EARS) + its Acceptance Criteria | Discussion/reasoning behind the decision |

The rule guiding this separation: **the requirement and its Acceptance Criteria always live together, in the same file** (`functional/`/`non-functional/`) — never in a separate artifact. Everything else (index, decisions, discussion) is metadata around the requirement, not the requirement itself.

## The 2 ID types

| ID | Lives in | What it is |
|---|---|---|
| `REQ-<ID>` | `functional/<feature>.md` | A system behavior, in EARS, with its Acceptance Criteria |
| `NFR-<ID>` | `non-functional/<attribute>.md` | A constraint/quality attribute, in EARS, with its Acceptance Criteria |

An `Open Question` is a formal, tracked artifact — but not one that lives in `requirements/`. It's an `OQ-<ID>` in `grill/open-questions.md` (see [Closing a Gap](#closing-a-gap-from-open-question-to-acceptance-criteria) above). A gap in a `REQ`/`NFR` only shows up here already resolved, as a Human Decision.

## Why this separation (instead of everything in one `requirements.md`)

- **Context rot**: a single giant file with all requirements, decisions, and discussions fills up the agent's context window and degrades response quality. Splitting by feature/attribute keeps each file small and focused.
- **Integrated Acceptance Criteria**: following Amazon Kiro / OpenSpec, the acceptance criterion always stays inside the requirement's own file — never isolated in another artifact.
- **Human Decisions separated from discussion**: `requirements.md` keeps a readable summary of *what* was decided (useful for a product overview); `grill/traceability.md` keeps the *how we got there* (useful for auditing, rarely needs to be read by the agent).
