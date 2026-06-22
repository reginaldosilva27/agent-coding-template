# claude-code-template

A reusable starting point that gives every project the same engineering discipline: **Spec-Driven
Development (SDD) + Test-Driven Development (TDD)**, a project **constitution**, ready-to-use Claude
Code **skills** and read-only **review agents**, **Codex** parity, and automated **GitHub Releases**
with a **Keep a Changelog** changelog.

Use it via GitHub's **"Use this template"** button, or `degit reginaldosilva/claude-code-template my-app`.

## What's inside

```
.specify/constitution.md     # the non-negotiable principles (edit per project)
specs/
  README.md                  # the SDD workflow
  _template/{spec,plan,tasks}.md
.claude/
  README.md                  # index of skills & agents
  skills/new-spec, grilling, verify-gates
  agents/code-reviewer, test-reviewer, docs-i18n-auditor
.codex/
  README.md
  prompts/new-spec, grilling, verify-gates, review-code, review-tests
.github/workflows/
  ci.yml                     # multi-stack gates (Python / Node / frontend presets)
  release.yml                # tag vX.Y.Z → GitHub Release with auto notes
CLAUDE.md / AGENTS.md        # twin agent guidance (keep in sync)
CHANGELOG.md                 # Keep a Changelog skeleton
docs/                        # architecture + development-workflow
```

## Bootstrap a new project (5 steps)

1. **Create from this template** ("Use this template" on GitHub, or `degit`).
2. **Fill the placeholders.** Search the repo for `{{...}}` and replace:
   `{{PROJECT_NAME}}`, `{{PROJECT_DESCRIPTION}}`, `{{DATE}}`, `{{MAINTAINER}}`,
   `{{PYTHON_DIR}}` / `{{NODE_DIR}}` / `{{FRONTEND_DIR}}`, `{{LINT_CMD}}` / `{{TEST_CMD}}` / etc.
   ```bash
   grep -rl '{{' . --exclude-dir=.git
   ```
3. **Pick your stack** in `.github/workflows/ci.yml` and `verify-gates` — keep the Python / Node /
   frontend jobs you use, delete the rest, fill the real commands.
4. **Trim the constitution** — keep §1–§6 (generic), keep or delete §7 (bilingual), and **add** any
   project-specific principles (e.g. an event-protocol contract, a single source of truth for a data
   model). Add project-specific skills under `.claude/skills` + mirror them in `.codex/prompts`.
5. **Write spec 000** — your first feature goes through `new-spec` like everything else.

## The discipline in one line

> No feature code without a spec (§1). No behavior change without a failing test first (§2). Not done
> until the gates are green (§3). One source of truth, docs follow code (§4). Nothing fake passed as
> real (§5). Agent guidance stays in sync (§6).

Full rules: [`.specify/constitution.md`](.specify/constitution.md) ·
workflow: [`specs/README.md`](specs/README.md) ·
how-to: [`docs/development-workflow.md`](docs/development-workflow.md).
