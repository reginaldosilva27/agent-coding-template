# Codex assets

This folder equips [OpenAI Codex](https://developers.openai.com/codex) to follow this project's
non-negotiable patterns (SDD + TDD, one source of truth, agent-guidance sync). The **canonical rules
live elsewhere** — [`../.specify/constitution.md`](../.specify/constitution.md),
[`../specs/README.md`](../specs/README.md), and [`../AGENTS.md`](../AGENTS.md). These prompts are thin
workflows that point at those; they don't restate the law.

This is the **Codex twin** of the Claude Code assets in [`../.claude/`](../.claude/). Same standards,
two front doors. Keep `AGENTS.md` ↔ `CLAUDE.md` and these prompts ↔ `.claude/skills`+`.claude/agents`
in sync when the rules change (constitution §6).

## Always-on instructions

Codex reads [`AGENTS.md`](../AGENTS.md) at the repo root automatically — the equivalent of `CLAUDE.md`
for Claude Code. Nothing to install.

## Prompts — invoke with `/<name>` in Codex

| Prompt | Use it for |
|---|---|
| `/new-spec` | Scaffold `specs/NNN-*/` before any feature code (constitution §1). |
| `/grilling` | The **clarify** engine — interview one question at a time to resolve a draft spec's open questions (or stress-test a plan) before code. |
| `/verify-gates` | Run the local CI mirror + cross-cutting constitution gates before a PR. |
| `/review-code` | Audit correctness, conventions, one-source-of-truth, scope (read-only). |
| `/review-tests` | Audit TDD: each AC mapped to a behavioral test, test-driven (read-only). |
| `/review-docs` | Audit docs-follow-code, CLAUDE.md ↔ AGENTS.md sync, en/pt parity (read-only). |

> Add any project-specific prompts here to mirror the agents in `.claude/agents`.

### Where Codex looks for prompts

Depending on your Codex version, custom prompts are read from the **global** prompt dir
(`$CODEX_HOME/prompts`, default `~/.codex/prompts/`) and/or a **project** `.codex/prompts/`. If your
Codex only sees the global dir, link them in once:

```bash
mkdir -p ~/.codex/prompts
ln -sf "$(pwd)/.codex/prompts/"*.md ~/.codex/prompts/
```

Either way the files double as plain Markdown checklists you can read directly.

## Recommended flow for a contribution

1. **Plan** → `/new-spec` and resolve the open questions before code.
2. **Build** → red → green → refactor.
3. **Self-review** → run the relevant `/review-*` prompts for the area you touched.
4. **Verify** → `/verify-gates`; open the PR only when it's all green.

This mirrors exactly what CI ([`.github/workflows/ci.yml`](../.github/workflows/ci.yml)) enforces.
