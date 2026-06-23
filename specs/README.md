# Specs — Spec-Driven Development (SDD)

This project is built **spec-first**: the intent is written and reviewed *before* the
code, and the spec stays the source of truth. SDD answers *what* and *why*; TDD proves
*it works*. The two interlock — acceptance criteria in a spec become failing tests.

Governing principles live in [`../.specify/constitution.md`](../.specify/constitution.md).

## Layout

```
.specify/
  constitution.md          # project principles (written once, amended on purpose)
specs/
  README.md                # this file — the workflow
  _template/               # copy this to start a feature
    spec.md                # WHAT + WHY + acceptance criteria
    plan.md                # HOW — technical approach, affected files
    tasks.md               # the work, as a TDD checklist
  NNN-feature-name/        # one folder per feature (001-, 002-, ...)
```

## Workflow

For each new feature:

1. **Specify** — copy `_template/` to `specs/NNN-feature-name/` and fill `spec.md`.
   Describe behavior and acceptance criteria; **no implementation detail yet**.
2. **Clarify** — resolve every open question before planning. This is the "grill me"
   step: run the `grilling` skill — it interviews you one question at a time (each with a
   recommended answer), folds the answers back into `spec.md`, and moves it to `clarified`.
3. **Plan** — fill `plan.md`: approach, files touched, data/contract impact, test strategy.
4. **Tasks** — fill `tasks.md`: ordered checklist, each item paired with its test.
5. **Implement (TDD)** — for each task: write the failing test (red) → implement
   (green) → refactor. Check the box.
6. **Verify** — all quality gates pass and every acceptance criterion maps to a passing test.

```
spec.md ──clarify──> plan.md ──> tasks.md ──TDD──> code + tests ──> gates green
  WHAT/WHY            HOW          WORK             red→green→refactor
```

## Numbering & status

- Folders are zero-padded, sequential: `001-`, `002-`, …
- Each `spec.md` carries a status: `draft → clarified → planned → in-progress → done`.
- Specs are an **append-only decision record** (like ADRs/RFCs): kept permanently, never
  renumbered or deleted. When a decision is replaced, write a new spec and mark the old
  one `superseded` with a link — don't edit history. The current *state* of the system
  lives in the docs (e.g. `docs/architecture.md`), not here.

## Index

The registry of every spec and where it stands. Keep this in sync when you add a spec or
move one along the lifecycle.

Legend: ✅ done · 🔧 in-progress · 📋 planned · 🔍 clarified · ✏️ draft

| # | Feature | Status |
|---|---|---|
| 001 | [Password reset via email link](001-password-reset/spec.md) — 📦 *example, delete for your project* | ✅ done |

## How SDD + TDD show up together

The acceptance criteria in `spec.md` are the bridge: each one is a testable statement,
and each becomes a test. A finished feature can point from every criterion to the test
that proves it.
