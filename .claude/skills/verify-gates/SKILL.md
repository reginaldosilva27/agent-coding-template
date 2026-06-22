---
name: verify-gates
description: Run the full local mirror of CI (the project's "definition of done") and report what passes/fails. Use before declaring any change done, before opening a PR, or when the user asks to verify. Mirrors .github/workflows/ci.yml plus the cross-cutting constitution gates.
---

A change is **done** only when these are green (constitution §3 "Quality gates"; mirrors `.github/workflows/ci.yml`). Run them, then report a concise pass/fail summary — do not claim done if anything is red; paste the failing output.

> **Fill in the commands for your stack** and delete the blocks that don't apply. Keep this
> file in lockstep with `.github/workflows/ci.yml` and the constitution's *Quality gates*.

## Python backend (from `{{PYTHON_DIR}}`)

```bash
ruff check .            # lint
ruff format --check .   # formatting (use `ruff format .` to fix)
pytest -q               # tests
```

## Node / TS backend (from `{{NODE_DIR}}`)

```bash
npm run lint            # eslint
npm test                # tests (vitest/jest)
npm run build           # tsc / build (type errors are gate failures)
```

## Frontend — React / TS / CSS (from `{{FRONTEND_DIR}}`)

```bash
npm run build           # tsc --noEmit + bundler build
npm test                # component/unit tests
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
