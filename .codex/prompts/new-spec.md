# /new-spec

Scaffold a Spec-Driven-Development spec **before any code**. This project is spec-first
(`.specify/constitution.md` §1) and test-first (§2), even when the request doesn't mention it.
If the user asked for code directly, **stop and remind them**, then do this.

Pure chores (docs, formatting, dep bumps) don't need a spec. When unsure, write one.

## Steps

1. **Next number** — list `specs/`, take the highest `NNN-` prefix + 1, zero-padded. Sequential.
2. **Copy template** — `cp -r specs/_template specs/NNN-feature-name/` (kebab-case, descriptive).
3. **Fill `spec.md`** — WHAT + WHY only, no implementation detail. Numbered, **testable** acceptance
   criteria (each becomes a failing test). Fill "Contract / data impact". Set `Status: draft`.
4. **Clarify** — resolve every open question *with the user*; empty that list to reach `clarified`.
5. **Fill `plan.md`** — HOW: approach, affected files, contract/data/i18n impact, test strategy (AC → test).
6. **Fill `tasks.md`** — ordered TDD checklist; each implement task preceded by its failing test.
7. Add the spec to the index in `specs/README.md`; move status along as you go.

Do **not** write production code from this prompt — finish the spec, then implement red → green →
refactor and run `/verify-gates` before calling anything done.
