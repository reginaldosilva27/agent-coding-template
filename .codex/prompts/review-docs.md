# /review-docs — documentation & agent-guidance audit

Codex counterpart of the Claude `docs-i18n-auditor` subagent. **Audit only — do not edit.**
Produce a precise findings report on documentation hygiene for this project (constitution §4
one-source-of-truth, §6 agent-guidance-in-sync, §7 bilingual).

## What to verify

1. **Docs follow code (§4).** If the change altered a behavior, contract, or schema that the docs
   describe (`README`, `docs/`, `CHANGELOG`), the docs moved in the **same** change. Flag stale docs
   and a missing `CHANGELOG` `[Unreleased]` entry for user-visible changes.
2. **Agent guidance in sync (§6).** If a rule/convention changed, `CLAUDE.md` and `AGENTS.md` say the
   same thing, and the `.claude/skills` ↔ `.codex/prompts` (and `.claude/agents` ↔ `.codex/prompts`)
   counterparts moved together. Flag any one-sided edit.
3. **Bilingual (§7 — only if the project is bilingual).** Every new user-facing string has both an
   `en` and a `pt` value. Scan the diff and the i18n source for English-only (or Portuguese-only)
   entries. Code, protocols, and proper nouns stay plain strings — don't flag those.

## How to work

- Start from `git diff` to see what changed and infer what docs/strings it touches.
- `grep` the i18n source(s) for keys added on only one side.
- Be specific: cite `file:line` and the exact missing doc paragraph / `pt` value / sync edit.

## Output

A short report: ✅ per invariant that holds, ❌ with `file:line` + the exact gap for each that
doesn't. End with a one-line verdict. Do not modify files.
