# ASD-STE100 Support — Design

## Problem

plsfix rewrites instruction documents against 12 writing principles, but none of those principles constrain *vocabulary* or *sentence grammar* the way ASD-STE100 (Simplified Technical English, a controlled-language standard built for maximum comprehension, clarity, standardization, and simplicity in technical writing) does. Two gaps exist:

1. plsfix's own instructional files (`SKILL.md`, `references/principles.md`, `help.md`) are not themselves written in STE, even though they are exactly the kind of procedural, agent-facing instructions STE is designed for.
2. Users who want plsfix to enforce STE-level language discipline on their *own* target documents have no way to ask for it — the 12 principles don't cover word choice or sentence-level grammar.

## Goals

- Add an opt-in `--ste` mode that layers STE rules on top of the existing 12-principle rewrite, for target documents where the user wants that discipline.
- Rewrite plsfix's own instructional files in STE, so the tool's own agent-facing instructions model the standard it can now apply.
- Keep this scoped: do not touch README.md or the community files (CONTRIBUTING.md, CODE_OF_CONDUCT.md, SECURITY.md, CHANGELOG.md) — their voice is marketing/community-facing, not instruction-following.

## Non-Goals

- Full ASD-STE100 compliance tooling (the real spec includes a ~900-word approved/not-approved dictionary and formal terminology management). plsfix will ship a condensed rule reference covering the core actionable grammar/style rules, not the full dictionary.
- Making STE the default. It stays opt-in via `--ste` — most documents plsfix handles (prompts, briefs, creative specs) aren't technical/procedural writing and would lose useful nuance under STE's constraints.
- Changing the 12 principles or their numbering. STE is additive, not a 13th principle, because it's a different kind of rule (dense grammar/vocabulary set vs. symptom-based structural/content check).

## Design

### 1. New `--ste` flag

Added to `SKILL.md`'s `## Flags` section, following the existing `--help`/`--version` pattern. Invoked as `/plsfix --ste` (flag-only, applies to the next document discussed) or inline with a normal request (e.g., "fix this `--ste`"). When active:

- **Step 1 (Diagnose):** after checking the 12 principles, also check the document against the STE rules in `references/ste100.md` — unapproved synonyms for a single meaning, sentences over ~20 words, passive voice, noun strings, gerunds used as adjectives, non-imperative procedure steps.
- **Step 2 (Rewrite):** apply STE fixes alongside principle fixes — one word per meaning throughout the document, short simple sentences, active voice, imperative mood for procedure steps.
- **Step 3 (Report):** add an **STE Compliance** section below the principle table, listing STE-specific changes (not numbered P1-P12, since they aren't principle violations).

If `--ste` is not passed, nothing changes — behavior matches today.

### 2. New reference file `references/ste100.md`

A condensed summary of ASD-STE100's core actionable writing rules — the ~15-20 rules a rewrite can mechanically check and apply, such as:

- One word, one meaning: don't vary vocabulary for the same concept
- Simple sentences: one main clause, roughly 20 words or fewer
- Active voice, simple verb tenses (mainly present and imperative)
- Procedure steps as direct commands ("Open the panel," not "The panel should be opened")
- No noun strings (chains of 3+ nouns stacked as modifiers)
- No gerunds used as adjectives
- No unnecessary synonyms for common connector words (e.g., prefer "use" over "utilize," "before" over "prior to")

This file is loaded only when `--ste` is active, mirroring how `references/principles.md` is loaded for the main workflow. It does not attempt to reproduce ASD-STE100's full approved/not-approved word dictionary.

### 3. Rewrite plsfix's own instructional files in STE

Rewrite these three files to follow the STE rules above, preserving their meaning and structure exactly (same sections, same flags, same workflow steps — just tightened grammar and vocabulary):

- `SKILL.md`
- `references/principles.md`
- `help.md`

No other files change voice. `README.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, and `CHANGELOG.md` are left as-is.

### 4. Versioning

Bump to the next minor version (new user-facing flag = a feature addition, per semver and this project's existing CHANGELOG pattern of minor bumps for flag additions like `--version`). Add a CHANGELOG entry under `[Unreleased]`. Follow the existing `/git-release` flow once implementation and tests (manual verification of the rewritten docs' clarity) are done.

## Testing / Verification

- Read through the rewritten `SKILL.md`, `references/principles.md`, and `help.md` to confirm meaning is unchanged and no workflow step, flag, or principle was dropped or altered in substance.
- Manually verify `--ste` is documented clearly enough that invoking it is unambiguous (what it does, what file it reads, what section it adds to the report).
- Spot-check `references/ste100.md` rules against authoritative ASD-STE100 summaries for accuracy.

## Risks

- **Condensed rule set risk:** a 15-20 rule summary is not the real ASD-STE100 dictionary-backed spec. Mitigation: document this limitation explicitly in `references/ste100.md`'s intro so users don't mistake it for full compliance tooling.
- **Voice drift in own-docs rewrite:** STE's imperative, short-sentence style could make `SKILL.md` feel terser than before. Mitigation: this is the intended effect (STE is being adopted deliberately here), and the rewrite must preserve all current meaning/behavior — reviewed via the testing step above.
