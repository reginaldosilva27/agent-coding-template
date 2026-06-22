---
name: verify-gates
description: Run the full local mirror of CI (the project's "definition of done") and report what passes/fails. Use before declaring any change done, before opening a PR, or when the user asks to verify. Mirrors .github/workflows/ci.yml plus the cross-cutting constitution gates.
---

A change is **done** only when these are green (constitution §3 "Quality gates"; mirrors `.github/workflows/ci.yml`). Run them, then report a concise pass/fail summary — do not claim done if anything is red; paste the failing output.

> Run each layer's gates from its own folder. Skip the layers the project doesn't use; adjust the
> tooling if a layer uses a different stack. Keep this file in lockstep with
> `.github/workflows/ci.yml` and the constitution's *Quality gates*.

## `backend/` — Python (FastAPI)

```bash
ruff check .            # lint
ruff format --check .   # formatting (use `ruff format .` to fix)
pytest -q               # tests
```

## `ai/` — Python (agents, prompts, RAG)

```bash
ruff check .            # lint
pytest -q               # tests (exercise real provider calls where feasible; skip if no key)
```

## `frontend/` — React + TypeScript + CSS

```bash
npm run lint            # eslint
npm test                # component/unit tests
npm run build           # tsc --noEmit + bundler build (type errors are gate failures)
```

## `infra/` — Terraform / IaC

```bash
terraform fmt -check -recursive   # formatting
terraform validate                # config is valid
```

## Cross-cutting gates (the ones tools won't fully catch)

Check these by inspecting the diff — they are constitution principles, not just CI steps:

- **§1 SDD** — the feature has a spec under `specs/` (and it's at least `planned`).
- **§2 TDD** — the change was driven by a test that failed first.
- **§4 one source of truth** — any contract/schema/config change moved its mirrors, types, and docs in the **same** change.
- **§6 agent sync** — if a rule changed, `CLAUDE.md` ↔ `AGENTS.md` and the `.claude`/`.codex` workflows moved together.
- **§7 bilingual** (if applicable) — every new user-facing string has both `en` and `pt`.

## Reporting

Summarize as a checklist with ✅/❌ per gate. On failure: name the gate, paste the relevant output, and propose the fix — don't silently move on. If a formatting check fails, offer to run the formatter.
