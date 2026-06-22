---
name: new-spec
description: Scaffold a new Spec-Driven-Development spec under specs/NNN-feature-name/ from the template. Use this whenever the user asks for a new feature, a behavior change, or any change to a public contract/schema — BEFORE writing any code. This project is spec-first (constitution §1); jumping to code is the #1 process violation.
---

This project is **spec-first (SDD)** and **test-first (TDD)** — non-negotiable, even when the user does not ask for it (`.specify/constitution.md` §1, §2). A new feature, a new user-facing behavior, or any change to a public contract/schema gets a **spec before code**. When unsure (gray zone), write the spec. Pure chores (docs, formatting, dependency bumps) don't need one.

If the user skipped the spec and asked for code directly, **stop and remind them**, then run this skill.

## Steps

1. **Find the next number.** List `specs/` and take the highest `NNN-` prefix + 1, zero-padded (`001`, `002`, …). Sequential, never reuse.
2. **Copy the template.** `cp -r specs/_template specs/NNN-feature-name/` (kebab-case, short, descriptive).
3. **Fill `spec.md`** — WHAT + WHY only. No implementation detail (if you name a file or a function, it belongs in `plan.md`). Numbered, **testable** acceptance criteria (each becomes a failing test). Fill the "Contract / data impact" block. Set `Status: draft`.
4. **Clarify** — run the `grilling` skill to resolve every "Open questions (clarify)" item *with the user*, one question at a time. Empty that list to reach `clarified`.
5. **Fill `plan.md`** — HOW: approach, affected files, contract/data/i18n impact, and a test strategy mapping **each AC → a test**.
6. **Fill `tasks.md`** — ordered TDD checklist; each implement task is preceded by the failing test that drives it.
7. Move status along as you go: `draft → clarified → planned → in-progress → done`. Add the spec to the index table in `specs/README.md`.

## Guardrails from the constitution (call these out in the spec when relevant)

- **§2 TDD** — acceptance criteria become failing tests before any implementation.
- **§4 one source of truth** — a contract/schema change names its single source of truth and lists everything that must move with it (mirrors, types, docs) in the same change.
- **§7 bilingual** (if the project is bilingual) — every new user-facing string ships `en` + `pt`. Note it in "User-facing behavior".

The canonical references are `specs/README.md` (the workflow) and `.specify/constitution.md`. Read them rather than re-deriving the rules.

Do **not** start writing production code from this skill — finish the spec, get it to `clarified`/`planned`, then implement red→green→refactor (use the `verify-gates` skill before calling anything done).
