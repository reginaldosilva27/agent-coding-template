# Claude Code assets

This folder equips [Claude Code](https://claude.com/claude-code) to follow this project's
non-negotiable patterns (SDD + TDD, one source of truth, agent-guidance sync). The
**canonical rules live elsewhere** — [`../.specify/constitution.md`](../.specify/constitution.md),
[`../specs/README.md`](../specs/README.md), and [`../CLAUDE.md`](../CLAUDE.md). These
skills/agents are thin workflows that point at those; they don't restate the law.

## Skills — invoke to *do* (`/skill-name` or ask Claude)

| Skill | When |
|---|---|
| `new-spec` | New feature / behavior change / contract change — **before any code** (constitution §1). Scaffolds `specs/NNN-*/` from the template. |
| `verify-gates` | Before "done"/PR — runs the local CI mirror + the cross-cutting constitution gates. |

> Add project-specific skills here (e.g. `add-stage`, `add-db-table`, `add-endpoint`) to
> encode the recurring multi-file rituals your codebase has. Mirror each in `.codex/prompts/`.

## Agents — spawn to *check* (read-only reviewers)

| Agent | Reviews |
|---|---|
| `code-reviewer` | correctness, conventions, one-source-of-truth, no-fake, scope. |
| `test-reviewer` | each AC mapped to a test, behavioral assertions, test-driven, real deps. |
| `docs-i18n-auditor` | docs follow code, CLAUDE.md ↔ AGENTS.md sync, en/pt parity (if bilingual). |

A good pre-PR routine: run the relevant `*` skill while building → `verify-gates` → spawn
`code-reviewer` + `test-reviewer` (+ `docs-i18n-auditor` if you touched docs/strings).

## Codex parity

For contributors using OpenAI Codex, the same standards are mirrored under
[`../AGENTS.md`](../AGENTS.md) (the twin of `CLAUDE.md`) and [`../.codex/prompts/`](../.codex/prompts/).
**Keep the two sides in sync:** `CLAUDE.md` ↔ `AGENTS.md`, and each skill/agent here ↔ its
`.codex/prompts/` counterpart, whenever a rule changes (constitution §6).
