# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

{{PROJECT_DESCRIPTION}}

## How we build here — SDD + TDD (non-negotiable)

This project is **spec-first (Spec-Driven Development)** and **test-first (Test-Driven Development)**,
and that applies to **every task — even when the user does not ask for it.** The intent is written
and reviewed *before* the code; acceptance criteria become failing tests. Two documents govern this,
and the constitution wins on any conflict:

- **`.specify/constitution.md`** — the non-negotiable principles + the quality gates + the amendment process.
- **`specs/README.md`** — the workflow: `specify → clarify → plan → tasks → implement (TDD) → verify`.
  `specs/_template/` is what you copy to start.

### New feature → write a spec first (stop and do this)
When the user asks for a new feature or a behavior change, **do not jump to code.** Create the spec,
and **if the user skipped it, remind them** before writing any code (the `new-spec` skill automates this):

1. Copy `specs/_template/` to `specs/NNN-feature-name/` (zero-padded, sequential).
2. Fill `spec.md` — WHAT + WHY + numbered, **testable** acceptance criteria. No implementation detail.
   Resolve every open question ("clarify") before planning.
3. Fill `plan.md` — HOW: approach, affected files, contract/data/i18n impact, and a test strategy that
   maps each acceptance criterion to a test.
4. Fill `tasks.md` — ordered TDD checklist; each implement task is preceded by the failing test that drives it.
5. Implement **red → green → refactor**, checking boxes, and move the spec's status along
   (`draft → clarified → planned → in-progress → done`).

### TDD always
Acceptance criteria (for a feature) or a reproducing case (for a bug) become a **failing test first**;
then code makes it pass; then refactor. Tests assert **behavior**, not implementation detail.

### Does this change need a spec?

| Change | Spec? | TDD? |
|---|---|---|
| New feature, new user-facing behavior, any public contract/schema change | **Yes** — full `spec → plan → tasks` | Yes |
| Bug fix · small adjustment · behavior-preserving refactor | **No** | **Yes** — failing regression test first, then fix |
| Docs · comments · formatting · dependency bumps · pure chores | No | n/a |

**Gray-zone rule:** when unsure, write the spec.

### Done means the gates are green
Run the `verify-gates` skill (mirror of `.github/workflows/ci.yml` + the constitution gates) before
declaring anything done. Fill the real commands into `verify-gates` and `ci.yml` for this project's stack.

## Commands

> Fill these in for your stack. Examples:

```bash
# Python (from {{PYTHON_DIR}})
ruff check . && ruff format --check . && pytest -q

# Node / TS (from {{NODE_DIR}})
npm run lint && npm test && npm run build

# Frontend (from {{FRONTEND_DIR}})
npm run build && npm test
```

## Architecture — the load-bearing ideas

<!-- Replace this section with the real architecture of THIS project: the modules, the
     contracts, the source(s) of truth, and the non-obvious invariants a contributor must
     not break. Keep it honest and current (constitution §4 — docs follow code). -->

## Conventions

- **SDD + TDD are mandatory** (see "How we build here"): a new feature gets a spec under `specs/`
  before code; a behavior change is driven by a failing test first. This holds even when the request
  doesn't mention it.
- **One source of truth (§4)** — each fact lives in one place; mirrors, types, and docs move with it.
- **Honesty (§5)** — nothing mocked is presented as real; misconfiguration fails fast.
- **Agent guidance in sync (§6)** — change a rule in `CLAUDE.md`, change it in `AGENTS.md` and the
  `.claude`/`.codex` workflows in the same commit.
- _(If bilingual — §7)_ any new user-facing text ships in both English and Portuguese.

## Docs

- `docs/architecture.md` — long-form walkthrough of the running system (kept in sync with the code).
- `docs/development-workflow.md` — how we build here (SDD + TDD), the contributor companion to the
  constitution and `specs/README.md`.
