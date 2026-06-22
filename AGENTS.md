# AGENTS.md

This file provides guidance to **OpenAI Codex** (and any other AI coding agent that reads
`AGENTS.md`) when working in this repository.

> **Twin of [`CLAUDE.md`](CLAUDE.md) — keep them in sync.** This project has one set of
> guidelines with two front doors: `CLAUDE.md` for Claude Code, `AGENTS.md` for Codex. They say
> the same thing on purpose. **If you change a rule in one, change it in the other in the same
> commit.** The non-negotiable source of truth above both is
> [`.specify/constitution.md`](.specify/constitution.md) — the constitution wins on any conflict.
> Codex-runnable workflows for the recurring tasks below live in [`.codex/prompts/`](.codex/prompts/).

## What this is

{{PROJECT_DESCRIPTION}}

## How we build here — SDD + TDD (non-negotiable)

This project is **spec-first** and **test-first**, even when the user does not ask for it. The intent
is written and reviewed *before* the code; acceptance criteria become failing tests. Two documents
govern this:

- **`.specify/constitution.md`** — the non-negotiable principles + quality gates + amendment process.
- **`specs/README.md`** — the workflow: `specify → clarify → plan → tasks → implement (TDD) → verify`.

### New feature → write a spec first (stop and do this)
When the user asks for a new feature or a behavior change, **do not jump to code** (the
`.codex/prompts/new-spec.md` prompt automates this):

1. Copy `specs/_template/` to `specs/NNN-feature-name/` (zero-padded, sequential).
2. Fill `spec.md` — WHAT + WHY + numbered, testable acceptance criteria. No implementation detail.
3. Fill `plan.md` — HOW: approach, affected files, contract/data/i18n impact, test strategy (AC → test).
4. Fill `tasks.md` — ordered TDD checklist; each implement task preceded by its failing test.
5. Implement red → green → refactor; move the spec's status along.

### TDD always
Acceptance criteria (feature) or a reproducing case (bug) become a failing test first; then code makes
it pass; then refactor. Tests assert behavior, not implementation detail.

### Does this change need a spec?

| Change | Spec? | TDD? |
|---|---|---|
| New feature, new user-facing behavior, any public contract/schema change | **Yes** | Yes |
| Bug fix · small adjustment · behavior-preserving refactor | **No** | **Yes** — regression test first |
| Docs · comments · formatting · dependency bumps · pure chores | No | n/a |

### Done means the gates are green
Run the `.codex/prompts/verify-gates.md` checklist (mirror of `.github/workflows/ci.yml` + the
constitution gates) before declaring anything done.

## Commands

> Fill these in for your stack (keep identical to `CLAUDE.md`).

## Architecture — the load-bearing ideas

<!-- Mirror the architecture section of CLAUDE.md. Keep both in sync (constitution §6). -->

## Conventions

- **SDD + TDD are mandatory.**
- **One source of truth (§4)** — mirrors, types, and docs move together.
- **Honesty (§5)** — nothing mocked is presented as real; misconfiguration fails fast.
- **Agent guidance in sync (§6)** — `CLAUDE.md` ↔ `AGENTS.md` ↔ `.claude`/`.codex` workflows.
- _(If bilingual — §7)_ any new user-facing text ships in both English and Portuguese.
