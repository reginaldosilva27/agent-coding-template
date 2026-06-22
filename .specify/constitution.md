# Project Constitution — {{PROJECT_NAME}}

> The non-negotiable principles every spec, plan, and change must respect.
> Written once, amended deliberately (see *Amendment process*). When a spec or a
> code change conflicts with a principle here, the constitution wins — or the
> principle is amended first, on purpose.

**Project:** {{PROJECT_DESCRIPTION}}

> This is a generic, reusable constitution. The principles below apply to **any**
> project. Add project-specific principles (e.g. an event-protocol contract, a
> single-source-of-truth visual model, provider rules) as numbered sections, and
> delete or amend any generic one that doesn't fit — but do it **on purpose**, in a
> PR, with a one-line rationale (see *Amendment process*).

---

## Core principles

### 1. Spec-first (SDD)
No feature code without a spec under `specs/`. The spec is the source of truth for
*what* and *why*; the plan for *how*; tasks for *the work*. This holds **even when the
request doesn't mention it.** See [`specs/README.md`](../specs/README.md).

### 2. Test-first (TDD)
Acceptance criteria (for a feature) or a reproducing case (for a bug) become a
**failing test before** implementation. Cycle: red → green → refactor. Tests assert
**behavior**, not implementation detail, so they survive refactors. Anything that
changes behavior is driven by a test; pure chores (docs, formatting, dep bumps) are not.

### 3. Done means the gates are green
A change is **done** only when the quality gates below pass — lint, tests, type-check/build
(see *Quality gates*). "Done" is never declared on red; if a gate fails, the output is
shown and the failure is fixed or surfaced, never silently skipped.

### 4. One source of truth — no forking, no drift
Each fact (a schema, a config default, a piece of prose) lives in exactly one place;
everything else derives from or links to it. Docs follow the code in the **same** change.
Don't duplicate a behavior across files when one shared definition will do.

### 5. Honesty — nothing fake passed off as real
Committed code does not fabricate, mock, or stub behavior and present it as real. External
dependencies are exercised for real in tests where feasible (provide keys/services as CI
secrets; mark and skip the dependent tests when unavailable). Misconfiguration **fails fast**
with a clear, typed error rather than silently degrading. *(A clearly-labelled demo/fixture
mode is allowed only if it is obviously labelled and replays real captured data — never
fabricated.)*

### 6. Agent guidance stays in sync
`CLAUDE.md` (Claude Code), `AGENTS.md` (Codex/others), and the `.claude/` + `.codex/`
workflows are **one set of rules with several front doors.** Change a rule in one, change
it in the others in the **same** commit. This constitution is the source of truth above all
of them.

### 7. (Optional) Bilingual user-facing text — en/pt
> Keep this section only for projects with a public-facing UI; delete it otherwise.

Every user-facing string the app renders ships in **both** English and Portuguese
(`{ en, pt }`, or the project's UI-strings file). Code, protocols, and proper nouns stay
plain strings. A feature is not done until its `pt` text exists alongside its `en` text.

---

## Quality gates (must pass before "done")

These mirror [`.github/workflows/ci.yml`](../.github/workflows/ci.yml). **Fill in the
commands for your stack** (delete the rows that don't apply):

- **Lint** — `{{LINT_CMD}}` (e.g. `ruff check .` · `npm run lint` · `eslint .`)
- **Format** — `{{FORMAT_CMD}}` (e.g. `ruff format --check .` · `prettier --check .`)
- **Tests** — `{{TEST_CMD}}` (e.g. `pytest -q` · `npm test` · `vitest run`)
- **Type-check / build** — `{{BUILD_CMD}}` (e.g. `tsc --noEmit && vite build` · `npm run build`)

Plus the cross-cutting gates from the principles above: spec exists for the feature (§1),
a failing test drove the change (§2), docs updated in the same change (§4), agent guidance
kept in sync (§6), and — if §7 applies — every new string is bilingual.

---

## Amendment process

This document changes only by intent, not by accident:

1. Propose the change in the PR / spec that needs it, with a one-line rationale.
2. Update this file in the **same** change.
3. If an existing spec now conflicts, reconcile it or mark it superseded.

---

*Ratified: {{DATE}} · Maintainer: {{MAINTAINER}}*
