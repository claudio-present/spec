<!--
  Copy this file into a new versioning/<NNN>-<slug>.md.
  <NNN> increments from the last file here; <slug> is a short kebab-case
  name for the change.

  Lifecycle: write it as Proposed, then apply the change to the living
  file and fill in "Applied" below, flipping status to Accepted. Only from
  Accepted onward is this file never edited again — see README.md for why.
-->
---
spec_id: <REQ-ID or NFR-ID or contract id being changed>
title: <short title for this change>
status: Proposed          # Proposed = written, not yet applied to the living file
                           # Accepted = applied — never edited again from here on
created: <YYYY-MM-DD>
applies_to: <path to the living file, e.g. requirements/functional/checkout.md#REQ-012>
applied_decisions:
  - HD-<ID>                # the Human Decision that authorized this — see requirements/requirements.md
---

# <Short title for this change>

## Ancestor (verbatim, as baselined)

> Quote the exact prior wording of the `REQ`/`NFR`/`AC` being changed,
> copied as it stood before this change — this is the only permanent copy
> of that wording once the living file is edited.

## Override (new behavior)

<The new wording, in EARS, exactly as it now appears — or will appear —
in the living file.>

## Rationale

<One or two sentences on why. The full discussion belongs in
requirements/traceability.md (or grill/traceability.md) under the linked
HD-<ID> — don't duplicate it here.>

## Applied

<!-- Fill in once applied, then flip status to Accepted above:
- Living file updated: <path>, version bumped to <X.Y.Z>
- Accepted: <YYYY-MM-DD>, commit <sha>
-->
