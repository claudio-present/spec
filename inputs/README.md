# Input Ingestion

Before a requirement exists, it has to come from somewhere. This phase defines what counts as a legitimate input to Spec-Driven Development (SDD), and how each one gets recorded — so a requirement can always be traced back to the raw material it was derived from, not just asserted from thin air.

In the Spec-as-Source paradigm, generation cannot be based on free text or assumption (*vibe coding*). For an AI to generate robust functional and logical contracts, inputs must be structured and modular — this also protects the system from **context rot** (the decline in an AI's reasoning as long-chat context accumulates), and keeps every input traceable back to its origin.

---

## Files in `inputs/`

```
inputs/
├── README.md       # this file: the 4 pillars
└── sources.md       # registry — one entry per consumed input, with justification
```

`sources.md` is not a lock file computed by a compiler — this repo has no such tooling. It's a registry filled in by whoever consumes an input, recording what was read and why.

---

## The 4 Pillars of Input

To build a synchronous, computable spec, four independent and modular categories of input feed the SDD engine:

```
                  ┌────────────────────────────────────────┐
                  │              INPUT SOURCES              │
                  └──────────────────┬─────────────────────┘
         ┌───────────────────────────┼───────────────────────────┐
         ▼                           ▼                           ▼
┌─────────────────┐         ┌─────────────────┐         ┌─────────────────┐
│ ANCESTRAL       │         │ MCPS & CONTEXT  │         │ CROSS-SPEC &    │
│ USER STORIES    │         │ GATEWAYS        │         │ HUMAN DECISIONS │
│ (pure intent)   │         │ (data / APIs)   │         │ (active HDs)    │
└─────────────────┘         └─────────────────┘         └─────────────────┘
```

### Pillar A: Ancestral User Stories (the business input)

**What it is:** the original, abstract definition of the value to be delivered to the end user (`As a... / I want... / So that...`).

**Governance rule:** user stories act as **immutable ancestors**. If the underlying business rules change later, the original story is never edited directly in the active file — instead, an *ancestral override* (a new story that inherits from and supersedes the previous one) is created, with full history kept in Git for continuous audit.

**Physical archiving:** once a story's override chain is settled, the superseded versions are moved into a dedicated archive folder rather than left cluttering the active input set — the same low-noise cleanup OpenSpec performs when a change proposal is closed.

> Source: [OpenSpec](https://github.com/Fission-AI/OpenSpec)

### Pillar B: MCPs & Context Gateways (the technical input)

**What it is:** controlled access to database schemas, existing API contracts, legacy code, or external technical documentation.

**Integration via MCP:** piling instructions and context into a single session degrades a model's reasoning and its ability to follow rules — the "curse of instructions." To avoid that, extensive manuals are not injected wholesale. The engine uses MCP connectors to expose on-demand query APIs — the specification agent consults the relevant documentation only at the exact moment it needs it to structure the conditional rule that depends on it, the same on-demand activation AWS Kiro's "Powers" use for domain-specific connectors (e.g. a payments API, a database schema) — loaded only when the domain is invoked, unloaded right after.

> Source: [Addy Osmani — "How to write a good spec for AI agents"](https://addyo.substack.com/p/how-to-write-a-good-spec-for-ai-agents)

### Pillar C: Cross-Spec Relations

**What it is:** the logical dependency between a new specification and the business or non-functional specifications already active in the repository.

**Structural rule:** every input explicitly declares which capabilities ([`requirements/functional/<feature>.md`](../requirements/functional/)) or platform constraints ([`requirements/non-functional/<attribute>.md`](../requirements/non-functional/)) it connects to, guaranteeing strict consistency of types, events, and data flows without duplicating text.

**Spec vs. steering:** an input either describes one task-scoped piece of behavior (a `functional/`/`non-functional/` requirement — a "Spec") or a project-wide rule that every feature must defer to and never contradict (a "Steering file" — in this repo, [`system-context/project-level.md`](../system-context/project-level.md)). An input that reads the same for every feature belongs to the latter; conflating the two is how a requirement quietly re-litigates a rule that was already settled at the project level.

### Pillar D: Human Decision Ledger (`HD-<ID>`)

**What it is:** the set of immutable architecture and product decisions already closed by the human team, after passing through the [Grill phase](../grill/README.md).

**Validation rule:** if a new business input directly conflicts with a decision already recorded in the ledger ([`HD-<ID>` in `requirements/requirements.md`](../requirements/requirements.md)) or with a project-wide invariant in [`system-context/project-level.md`](../system-context/project-level.md#6-global-invariants), that's a synchronous-integrity error — it forces the team to re-open the conflict, not silently override what's already recorded. Re-opening it means raising a new `OQ-<ID>` in [`grill/open-questions.md`](../grill/open-questions.md), never editing the closed `HD-<ID>`, its archived record in [`grill/traceability.md`](../grill/traceability.md), or a project-level invariant directly.

---


## From Pillar to `sources.md`

Every input actually consumed — regardless of which pillar it comes from — gets one entry in `sources.md`: what it is, where it came from, which requirement it feeds, and why it was consumed (or, for a related input that was deliberately left out, why not). See [`sources.md`](./sources.md) for the entry format.

---

## Open design questions

- Whether `sources.md` should eventually be replaced by a machine-generated lock file (hash-based, auto-detecting changed/new/removed inputs) instead of a hand-maintained registry — [`OQ-001`](../grill/open-questions.md).
- Whether MoSCoW belongs at this input-triage stage, tagging a raw user story before decomposition, even though it was dropped from `requirements/` itself — [`OQ-002`](../grill/open-questions.md).

Neither is decided here — see the linked entries in `grill/open-questions.md`.
