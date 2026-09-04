<!--
  SKELETON — PROJECT LEVEL. One per project, commonly copied in as
  AGENTS.md / GROUNDING.md / constitution.md at the repo root. Replace every
  <placeholder>.

  Belongs here only if it would read the same in every feature. The moment a
  sentence names one feature's own component, DTO or boundary, it belongs in
  that feature's system-context/feature-level.md instead.

  Read by every feature-level.md — they reference it, never repeat it.
-->

# System Context — Project

`<Project Name>` · governs every `system-context/feature-level.md` under this project.

## 1. Project Identity

<what the system is, in one paragraph: platforms, runtime, domain>

## 2. Folder / Module Pattern

<the one folder shape every feature follows, annotated>

## 3. Composition Mechanism

<how a feature wires into the running system, and any ordering constraint>

## 4. Cross-Component Communication — sanctioned channels only

| Channel | When to use it | Owned by |
|---|---|---|
| `<channel name>` | `<kind of interaction>` | `<owner>` |

**No third channel.** A feature needing a direct call, shared state, or a new
global outside this list is making an architectural decision — it must say
so explicitly, never introduce it silently.

## 5. State Management & Routing

<the one stack-level choice every feature's own state/routing is built on
top of, never a different one>

## 6. Global Invariants

- **Identity:** `<how every entity gets its id>`
- **Persistence:** `<the one path data takes to storage>`
- **Security / access control:** `<the global rule — no implicit default>`

## 7. Global Mechanical Controls

- `<lint/style rule>`
- `<generated-file rule — which paths are never hand-edited>`
- `<dependency-version rule, if drift here has caused a real incident>`

## 8. Global QA Isolation Rule

<what a human or agent may/may not do to a test or its golden output, and
who actually generates that output>

## What a feature-level.md may assume without re-deriving

Everything in §§2–8. A feature that needs to deviate from one of them must
say so as an explicit, flagged exception — never silently.
