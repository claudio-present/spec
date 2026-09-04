# requirements.md

## What makes a good requirement

A good requirement is one behavior, stated so plainly you could hand it to someone else to test.

- **One statement, one `SHALL`/`MUST`.** If a requirement has three "and also" clauses, it's really three requirements. Split them.
- **Observable.** Someone outside the code should be able to tell whether it holds. "The system SHALL show an error banner when the upload exceeds 10 MB" is observable. "The system SHALL handle large uploads gracefully" is not.
- **The right strength.** OpenSpec uses the RFC 2119 keywords, and they mean different things:

  | Keyword | Meaning |
  |---------|---------|
  | `MUST` / `SHALL` | A hard requirement. Non-negotiable. |
  | `SHOULD` | A strong recommendation, with room for a justified exception. |
  | `MAY` | Genuinely optional. |

  Reach for `MUST`/`SHALL` by default. Use `SHOULD` only when you truly mean "unless there's a good reason not to."

The test for a requirement: *could a tester who's never seen the code tell whether it passed?* If not, it needs sharpening.

> Source: [OpenSpec — Writing Specs](https://github.com/Fission-AI/OpenSpec/blob/main/docs/writing-specs.md)

---

## Technological Independence

As advocated by Spec Kit, Spec-Driven Development (SDD) is founded on the premise that the engineering process should not be tied to specific stacks, languages, or frameworks.

---

## EARS Syntax as a Logical Predicate Engine

Free-form natural language writing is inherently imprecise, which leads AI agents into error. Adopting the EARS syntax (Easy Approach to Requirements Syntax) acts as a lightweight logical constraint on the text.

> Source: [EARS — Alistair Mavin](https://alistairmavin.com/ears/)

### The 5 Structural Patterns

The syntax groups system behaviors into rigid patterns that use specific keywords, easy to translate into programming logic:

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

## MoSCoW Prioritization

According to the GSD Core repository's documentation, the overwhelming majority of AI agent workflows fail at scale due to **context rot** (the decline in response quality and accuracy that occurs as the LLM's context window fills up).

> Source: [open-gsd/gsd-core](https://github.com/open-gsd/gsd-core)

MoSCoW Prioritization, a methodology originally developed by Dai Clegg at Oracle, divides business rules into rigid categories based on the product's level of need:

- **Must-have** — mandatory
- **Should-have** — important but not vital
- **Could-have** — nice-to-have
- **Will-not-have** — out of immediate scope

Its main benefit in traditional engineering is preventing *scope creep*.

Crossing these two realities, we see that the MoSCoW embedded in `requirements.md`'s metadata acts as a context-control mechanism for the AI.

> Source: [MoSCoW Prioritization — ProductPlan](https://www.productplan.com/glossary/moscow-prioritization)

---

## Open Questions

Open Questions are handled in `requirements.md` in a transient, blocking way (identical to `/speckit.clarify`). The presence of active questions signals that the requirement is "gapped" (incomplete), preventing the compiler from moving on to code generation.

> Source: [github/spec-kit](https://github.com/github/spec-kit)

Once answered, the human clarification decisions are translated into clean acceptance criteria written in EARS and injected into `requirements.md`.

> Source: [Business Requirement to Functional Spec — Analyst Engineering](https://www.analystengineering.com/articles/business-requirement-to-functional-spec)

The detailed record of logical discussions and product meeting notes is moved permanently to the standalone `traceability.md` file, protecting the generation agent's active context from GSD Core's context rot.

> Source: [open-gsd/gsd-core](https://github.com/open-gsd/gsd-core)

---

## Acceptance Criteria

In the Spec-as-Source paradigm (and in the practices of leading state-of-the-art tools such as Amazon Kiro and OpenSpec), Acceptance Criteria must **NOT** go into a separate artifact.

In fact, they must be fully integrated within the requirements file itself (`requirements.md` or the `/functional/` folder).

---

# Structure of `requirements/`

This section explains *how* the `requirements/` folder is organized and *why*. The rules above explain how to write a good requirement (EARS, MoSCoW, RFC 2119); this section explains where everything actually lives and how it flows between files.

## File tree

```
requirements/
├── README.md                    # this file: writing rules + folder structure
├── requirements.md              # product index + Human Decisions
├── traceability.md              # decision history/reasoning
├── functional/
│   ├── _TEMPLATE.md             # template for a new capability/feature
│   └── <feature>.md             # one file per capability (REQ-*)
└── non-functional/
    ├── _TEMPLATE.md             # template for a new quality attribute
    └── <attribute>.md           # one file per attribute (NFR-*), e.g. performance.md, security.md
```

## Role of each file

| File | Contains | Does not contain |
|---|---|---|
| `README.md` | Writing rules (EARS, MoSCoW, RFC 2119) + this structure | Concrete requirements |
| `requirements.md` | Index (links + product-level MoSCoW) and `Human Decisions` (the "why" behind each decision already made) | Requirements, Acceptance Criteria |
| `functional/<feature>.md` | `REQ-<ID>` (behavior, in EARS) + its Acceptance Criteria | Discussion/reasoning behind the decision |
| `non-functional/<attribute>.md` | `NFR-<ID>` (constraint/quality, in EARS) + its Acceptance Criteria | Discussion/reasoning behind the decision |
| `traceability.md` | Detailed reasoning, meeting notes, arguments behind each decision | Requirements, Acceptance Criteria |

The rule guiding this separation: **the requirement and its Acceptance Criteria always live together, in the same file** (`functional/`/`non-functional/`) — never in a separate artifact. Everything else (index, decisions, discussion) is metadata around the requirement, not the requirement itself.

## The 2 ID types

| ID | Lives in | What it is |
|---|---|---|
| `REQ-<ID>` | `functional/<feature>.md` | A system behavior, in EARS, with its Acceptance Criteria |
| `NFR-<ID>` | `non-functional/<attribute>.md` | A constraint/quality attribute, in EARS, with its Acceptance Criteria |

There is no formal "Open Question" artifact in this structure — a gap in a `REQ`/`NFR` is resolved directly (outside the file, in conversation/discussion), and the result comes in already resolved, as described below.

## Why this separation (instead of everything in one `requirements.md`)

- **Context rot**: a single giant file with all requirements, decisions, and discussions fills up the agent's context window and degrades response quality. Splitting by feature/attribute keeps each file small and focused.
- **Integrated Acceptance Criteria**: following Amazon Kiro / OpenSpec, the acceptance criterion always stays inside the requirement's own file — never isolated in another artifact.
- **Human Decisions separated from discussion**: `requirements.md` keeps a readable summary of *what* was decided (useful for a product overview); `traceability.md` keeps the *how we got there* (useful for auditing, rarely needs to be read by the agent).
