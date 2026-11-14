# ASD-STE100 Support — Design

## Problem

plsfix rewrites instruction documents against 12 writing principles, but none of those principles constrain *vocabulary* or *sentence grammar* the way ASD-STE100 (Simplified Technical English, the ASD specification for maintenance documentation in a controlled language, built for maximum comprehension, clarity, standardization, and simplicity) does. Two gaps exist:

1. plsfix's own instructional files (`SKILL.md`, `references/principles.md`, `help.md`, and their cross-platform mirror copies under `skills/plsfix/`) are not themselves written in STE, even though they are exactly the kind of procedural, agent-facing instructions STE is designed for.
2. Users who want plsfix to enforce STE-level language discipline on their *own* target documents have no way to ask for it — the 12 principles don't cover word choice or sentence-level grammar.

## Goals

- Add an opt-in `--ste` flag that layers STE rules on top of the existing 12-principle rewrite, for target documents where the user wants that discipline.
- Rewrite plsfix's own instructional files (and their cross-platform mirrors) in STE, so the tool's own agent-facing instructions model the standard it can now apply.
- Keep this scoped: do not touch `README.md` or the community files (`CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, `CHANGELOG.md`) — their voice is marketing/community-facing, not instruction-following.

## Non-Goals

- Full ASD-STE100 compliance tooling (the real spec includes a roughly 900-word approved/not-approved dictionary and formal terminology management). plsfix ships a condensed rule reference covering 16 core actionable grammar/style rules (enumerated in Design §2), not the full dictionary.
- Making STE the default. It stays opt-in via `--ste` — most documents plsfix handles (prompts, briefs, creative specs) aren't technical/procedural writing and would lose useful nuance under STE's constraints.
- Changing the 12 principles or their numbering. STE is additive, not a 13th principle, because it's a different kind of rule (dense grammar/vocabulary set vs. symptom-based structural/content check).
- Fixing the pre-existing gap where `help.md`'s `FLAGS` section documents `--help` but not `--version` (confirmed by reading `help.md` against `SKILL.md`). That's a pre-existing omission unrelated to this feature; this spec only requires adding `--ste` to that same `FLAGS` section, not auditing or completing the rest of it.

## Design

### 1. New `--ste` flag

Added to `SKILL.md`'s `## Flags` section, as a new subsection after `--version`, and mirrored into `help.md`'s `FLAGS` and `USAGE` sections (see Design §3 for exactly which files this touches).

**How it differs from `--help`/`--version`:** those two are terminal flags — per `SKILL.md` lines 28-45, each one short-circuits the entire workflow and never reaches Steps 1-4. `--ste` is the opposite: it is additive and non-terminal. It does not replace or skip any step; it adds STE checks inside the existing Steps 1-3 alongside the normal 12-principle pass, and Step 4 (Present) runs unchanged. The new `## Flags` subsection for `--ste` must state this distinction explicitly so an implementer doesn't model it as another early-exit branch.

**Invocation syntax:** `--ste` is accepted in either position relative to the document argument — both `/plsfix --ste <path-to-document>` and `/plsfix <path-to-document> --ste` are valid and equivalent. Document both literal forms in `SKILL.md`'s `## Flags` entry and in `help.md`'s `USAGE` and `FLAGS` blocks:

- `USAGE` line: `/plsfix <path-to-document> --ste   Improve the document, enforcing ASD-STE100 language rules`
- `FLAGS` line, in the same style as the existing `--help    Show this help message without making any changes`: `--ste     Also enforce ASD-STE100 (Simplified Technical English) language rules`

**Effect on the workflow when `--ste` is present:**

- **Step 1 (Diagnose):** after checking the 12 principles, also check the document against the 16 STE rules listed in `references/ste100.md` (same enumeration as Design §2 below — the two lists must match exactly, there is no separate or shorter list for Step 1). Note every STE violation with its location and which numbered STE rule (R1-R16, see §2) it breaks, the same way principle violations are noted.
- **Step 2 (Rewrite):** apply STE fixes alongside principle fixes — one word per meaning throughout the document, short simple sentences, active voice, imperative mood for procedure steps, and the rest of R1-R16.
- **Step 3 (Report):** add a new table, titled **STE Compliance**, using this exact schema, placed immediately after the **Principles Not Triggered** line and after the **Split Candidates** section if one was rendered (both of those already sit "below the principle table" per the current Step 3 rules, so STE Compliance is the last section before Step 4):

  ```
  ## STE Compliance

  | # | Location | STE Rule | Before | After | Rationale |
  |---|----------|----------|--------|-------|-----------|
  | 1 | Section 2, para 3 | R1: One word, one meaning | Document used "commence," "initiate," and "begin" interchangeably for the same action | Standardized on "start" throughout | Mixed terms for one action force the reader to confirm they're the same step |
  | 2 | Section 4 | R5: Procedure steps as direct commands | "The configuration file should be edited by the user" | "Edit the configuration file" | Passive description read as optional; the imperative reads as a required step |
  ```

  This matches the six-column schema of the existing `plsfix Change Report` table (`# | Location | Principle | Before | After | Rationale`) exactly, substituting `STE Rule` for `Principle` — kept consistent rather than dropped, so the two tables read the same way.

  If no STE violations were found, omit the **STE Compliance** section entirely (same silence rule as the existing "no violations, no row" convention for principles). This section only appears when `--ste` was active for the run.

If `--ste` is not passed, nothing changes — behavior matches today exactly.

### 2. New reference file `references/ste100.md`

A condensed summary of ASD-STE100's writing rules, condensed from the ASD-STE100 specification's "Writing Rules" chapter (publicly summarized by the Simplified Technical English Maintenance Group at ste100.org and by the Wikipedia article "Simplified Technical English"). It covers the following 16 rules, numbered R1-R16 — this exact numbered list is authoritative and is the same list Design §1's Step 1 checks against (no separate or abridged list anywhere else):

1. **R1 — One word, one meaning:** don't vary vocabulary for the same concept (e.g., always "start," never alternate with "commence"/"initiate").
2. **R2 — Simple sentences:** one main clause per sentence, roughly 20 words or fewer.
3. **R3 — Active voice:** rewrite passive constructions ("the file is opened by the user") as active ("the user opens the file" / "open the file").
4. **R4 — Simple verb tenses:** prefer present tense and imperative; avoid compound/conditional tenses (future perfect, "would have," etc.).
5. **R5 — Procedure steps as direct commands:** write instructions as imperative verbs ("Open the panel"), not descriptions ("The panel should be opened").
6. **R6 — No noun strings:** avoid chains of 3+ nouns stacked as modifiers (e.g., "data validation error report" → "report of data validation errors").
7. **R7 — No gerunds as adjectives:** avoid "-ing" forms modifying a noun ("the operating procedure" → "the procedure for operation," or reword around it).
8. **R8 — No gerunds as nouns where a simpler noun exists:** avoid "the opening of the file" when "opening the file" or a plain noun works.
9. **R9 — One instruction per sentence:** don't chain two or more commands with "and"; split into separate sentences or numbered steps.
10. **R10 — Avoid ambiguous pronouns:** don't use "it," "this," or "that" unless the referent is the immediately preceding noun; otherwise repeat the noun.
11. **R11 — Avoid stacked past-participle modifiers:** e.g., "the previously validated submitted form" → reword to separate the modifiers.
12. **R12 — Avoid vague quantifiers:** replace "some," "several," "a few" with a specific count or range when one is knowable.
13. **R13 — Keep articles ("a," "the"):** don't drop articles in a terse, telegraphic style; STE keeps normal English article usage.
14. **R14 — Avoid unnecessary synonyms for common connector/function words:** prefer the plain form — "use" not "utilize," "before" not "prior to," "about" not "approximately," "if" not "in the event that."
15. **R15 — One topic per paragraph:** keep each paragraph focused on a single idea (mirrors plsfix's existing P2 "one ask per section," applied at the paragraph level).
16. **R16 — Spell out ambiguous abbreviations on first use:** if an abbreviation isn't universally unambiguous in context, write it in full the first time, then abbreviate.

The file's intro must state plainly that this is a condensed, 16-rule practical summary — not the full ASD-STE100 specification with its approved/not-approved word dictionary — so users don't mistake `--ste` for full certification-grade compliance tooling.

This file is loaded only when `--ste` is active, mirroring how `references/principles.md` is loaded for the main workflow.

### 3. Files touched, including cross-platform mirrors

This repo keeps byte-identical mirror copies of `SKILL.md`, `help.md`, and `references/principles.md` under `skills/plsfix/` for Codex/Gemini/Antigravity (confirmed identical today; `GEMINI.md` directly `@`-includes `skills/plsfix/SKILL.md` and `skills/plsfix/references/principles.md`, and README.md states all four platforms share the same `SKILL.md` format). Every change below must be applied to both the top-level file and its `skills/plsfix/` mirror, kept byte-identical, including the brand-new `references/ste100.md` (which also needs a copy at `skills/plsfix/references/ste100.md`):

**`GEMINI.md`'s static includes stay unchanged.** `GEMINI.md` currently only `@`-includes `skills/plsfix/SKILL.md` and `skills/plsfix/references/principles.md` — it does not include `skills/plsfix/help.md`, even though `help.md` exists and is read on demand (via the `--help` flag), not preloaded. `references/ste100.md` follows that same `help.md` precedent: it's read on demand only when `--ste` is active, so `GEMINI.md` does **not** get a new `@`-include line for it. The file still needs its `skills/plsfix/` mirror copy (for Codex/Gemini/Antigravity's on-demand reads to find it), it just isn't added to `GEMINI.md`'s always-loaded set.

| File | Change |
|------|--------|
| `SKILL.md` + `skills/plsfix/SKILL.md` | Add the `--ste` flag subsection (Design §1); rewrite the rest of the file's prose into STE (Design §4) |
| `help.md` + `skills/plsfix/help.md` | Add `--ste` to `USAGE` and `FLAGS` (Design §1); rewrite the rest of the file's prose into STE (Design §4) |
| `references/principles.md` + `skills/plsfix/references/principles.md` | Rewrite into STE (Design §4) — no flag-related content here, this file only gains the STE voice pass |
| `references/ste100.md` + `skills/plsfix/references/ste100.md` | New file (Design §2) — written in STE itself from the start, so it doesn't need a separate rewrite pass |

The flag-list additions (to `SKILL.md` and `help.md`) and the STE-voice rewrite (to the prose of those same two files, plus `references/principles.md`) are two distinct edits to the same files — the flag content is new material describing a new capability; the voice rewrite is a style pass over existing content. Apply both in either order, but both are required before this ships.

### 4. Rewrite plsfix's own instructional files in STE

Rewrite the prose of `SKILL.md`, `references/principles.md`, and `help.md` (and their mirrors, per §3's table) to follow the R1-R16 rules in Design §2, preserving meaning and structure exactly — same sections, same workflow steps, same principle names and numbering, same change-report schema. Only the grammar and vocabulary tighten; no content is added, removed, or reordered beyond what §1 and §3 already specify as new.

No other files change voice. `README.md`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `SECURITY.md`, and `CHANGELOG.md` are left as-is.

### 5. Versioning

This is a new, user-invokable flag/capability — not a refinement of existing default behavior — so it warrants a **minor** version bump, consistent with the 1.1.0 release ("Add --version flag support"). This is distinct from the 1.1.1 patch release, which only extended what the existing default diagnosis step already checked for (no new flag, no new opt-in capability); that precedent doesn't apply here.

On release, bump the version in every manifest that carries one, matching current project convention (confirmed in `.codex-plugin/plugin.json`, `gemini-extension.json`, and git history: commits `3877c7c` "Bump version to v1.1.0" and `c98f330` "Bump version to v1.1.1" both touch these two manifest files, establishing that each release bump updates them in lockstep with the release tag):

- `.codex-plugin/plugin.json` — `"version"` field
- `gemini-extension.json` — `"version"` field
- Any further manifest with a `"version"` field introduced between now and release (re-check at release time, same as the existing `--version` flag logic in `SKILL.md` does when it looks for a manifest to read)

Add a CHANGELOG entry under `[Unreleased]` describing the new `--ste` flag and the STE-voice rewrite of the core instructional files. Follow the existing `/git-release` flow once implementation and verification (Design §6) are done.

## Testing / Verification

- **Behavioral check (primary):** actually invoke `/plsfix --ste` against a sample document that contains clear STE violations (passive voice, a noun string, a non-imperative procedure step, inconsistent synonyms for one concept) and confirm the resulting Diagnose/Rewrite/Report matches Design §1 exactly — violations are found and tagged with the correct R-number, fixes are applied, and the **STE Compliance** table appears in the specified position with the specified schema. Also run `/plsfix` without `--ste` against the same document and confirm no STE-related content appears, to verify the flag is truly opt-in.
- **Read-through check:** read the rewritten `SKILL.md`, `references/principles.md`, and `help.md` (top-level copies) to confirm meaning is unchanged and no workflow step, flag, or principle was dropped or altered in substance.
- **Mirror parity check:** run a diff between each top-level file and its `skills/plsfix/` mirror (`SKILL.md`, `help.md`, `references/principles.md`, and the new `references/ste100.md`) and confirm zero differences, matching this repo's existing sync convention.
- **Rule spot-check:** spot-check the R1-R16 list in `references/ste100.md` against the ASD-STE100 writing-rules summaries named in Design §2 for accuracy.

## Risks

- **Condensed rule set risk:** a 16-rule summary is not the real ASD-STE100 dictionary-backed spec. Mitigated by the explicit disclaimer required in `references/ste100.md`'s intro (Design §2) — this names the limitation to users rather than only noting it here.
- **Voice drift in own-docs rewrite:** STE's imperative, short-sentence style could make `SKILL.md` feel terser than before. This is the intended effect (STE is being adopted deliberately here); the read-through check in Testing/Verification guards against accidental meaning loss.
- **Mirror drift:** forgetting to update one of the four `skills/plsfix/` mirror files would silently break Codex/Gemini/Antigravity parity. Mitigated by making the mirror-parity diff check an explicit, required verification step rather than an afterthought.
