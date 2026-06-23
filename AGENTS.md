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
When the user asks for a new feature or a behavior change, **do not jump to code** — and **if the
user skipped it, remind them** before writing any code (the `.codex/prompts/new-spec.md` prompt
automates this):

1. Copy `specs/_template/` to `specs/NNN-feature-name/` (zero-padded, sequential).
2. Fill `spec.md` — WHAT + WHY + numbered, **testable** acceptance criteria. No implementation detail.
   Resolve every open question ("clarify") before planning.
3. Fill `plan.md` — HOW: approach, affected files, contract/data/i18n impact, test strategy (AC → test).
4. Fill `tasks.md` — ordered TDD checklist; each implement task preceded by its failing test.
5. Implement red → green → refactor, checking boxes; move the spec's status along
   (`draft → clarified → planned → in-progress → done`).

### TDD always
Acceptance criteria (feature) or a reproducing case (bug) become a failing test first; then code makes
it pass; then refactor. Tests assert behavior, not implementation detail.

### Does this change need a spec?

| Change | Spec? | TDD? |
|---|---|---|
| New feature, new user-facing behavior, any public contract/schema change | **Yes** | Yes |
| Bug fix · small adjustment · behavior-preserving refactor | **No** | **Yes** — regression test first |
| Docs · comments · formatting · dependency bumps · pure chores | No | n/a |

**Gray-zone rule:** when unsure, write the spec.

### Done means the gates are green
Run the `.codex/prompts/verify-gates.md` checklist (mirror of `.github/workflows/ci.yml` + the
constitution gates) before declaring anything done. Fill the real commands into
`.codex/prompts/verify-gates.md` and `ci.yml` for this project's stack.

## Commands

> Default layered architecture: `backend/` · `frontend/` · `ai/` · `infra/`. Run each layer's gates
> from its own folder. Keep identical to `CLAUDE.md` (§6). Adjust tooling per stack; delete unused layers.

```bash
# backend/ — Python (FastAPI)
ruff check . && ruff format --check . && pytest -q

# ai/ — Python (agents, prompts, RAG)
ruff check . && pytest -q

# frontend/ — React + TypeScript + CSS
npm run lint && npm test && npm run build

# infra/ — Terraform / IaC
terraform fmt -check -recursive && terraform validate
```

## Architecture — the load-bearing ideas

<!-- Mirror the architecture section of CLAUDE.md. Keep both in sync (constitution §6). -->

## Working style — behavioral guardrails

> Behavioral nudges (bias toward caution over churn), not CI-gated. The constitution (§1–§7) is
> the law above them. Mirror of CLAUDE.md (§6). _(Adapted from Andrej Karpathy's coding guidelines.)_

- **Simplicity first.** Minimum code that solves the problem — nothing speculative: no unrequested
  features, no abstraction for single-use code, no configurability nobody asked for, no error
  handling for impossible states. If 200 lines could be 50, rewrite it.
- **Surgical changes.** Touch only what the request requires; match the surrounding code; don't
  refactor what isn't broken. Remove only the orphans **your** change created; pre-existing dead
  code you mention, not delete. Every changed line traces to the request.
- **Think before coding.** State assumptions; surface multiple interpretations instead of silently
  picking one; push back when a simpler approach exists. For spec ambiguity, that's the `grilling`
  (clarify) step.

## Conventions

- **SDD + TDD are mandatory** (see "How we build here"): a new feature gets a spec under `specs/`
  before code; a behavior change is driven by a failing test first. This holds even when the request
  doesn't mention it.
- **One source of truth (§4)** — mirrors, types, and docs move together.
- **Honesty (§5)** — nothing mocked is presented as real; misconfiguration fails fast.
- **Agent guidance in sync (§6)** — `CLAUDE.md` ↔ `AGENTS.md` ↔ `.claude`/`.codex` workflows.
- _(If bilingual — §7)_ any new user-facing text ships in both English and Portuguese.

## Docs

- `docs/architecture.md` — long-form walkthrough of the running system (kept in sync with the code).
- `docs/development-workflow.md` — how we build here (SDD + TDD), the contributor companion to the
  constitution and `specs/README.md`.
