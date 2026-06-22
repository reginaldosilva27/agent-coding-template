# /verify-gates

Run the local mirror of CI (the project's definition of done) and report pass/fail. Mirrors
`.github/workflows/ci.yml` + the constitution gates (§3). Do not claim done on red; paste failing output.

> Run each layer's gates from its own folder; skip the layers the project doesn't use.

## `backend/` — Python (FastAPI)

```bash
ruff check . && ruff format --check . && pytest -q
```

## `ai/` — Python (agents, prompts, RAG)

```bash
ruff check . && pytest -q
```

## `frontend/` — React + TypeScript + CSS

```bash
npm run lint && npm test && npm run build
```

## `infra/` — Terraform / IaC

```bash
terraform fmt -check -recursive && terraform validate
```

## Cross-cutting gates (inspect the diff)

- **§1 SDD** — the feature has a spec under `specs/`.
- **§2 TDD** — a test that failed first drove the change.
- **§4 one source of truth** — contract/schema/config changes moved mirrors, types, and docs together.
- **§6 agent sync** — `CLAUDE.md` ↔ `AGENTS.md` and `.claude`/`.codex` workflows moved together.
- **§7 bilingual** (if applicable) — every new user-facing string has both `en` and `pt`.

## Report

Checklist with ✅/❌ per gate. On failure: name the gate, paste the output, propose the fix.
