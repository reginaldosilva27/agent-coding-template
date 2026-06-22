# /review-code

Read-only review of the current change against this project's conventions and constitution.
**Audit only — do not edit.** Produce a precise findings report.

## Verify

1. **Correctness** — does what its spec/PR says; edge cases and error paths handled; misconfiguration
   fails fast rather than degrading silently (§5).
2. **Conventions** — reads like the surrounding code (naming, structure, error handling, async). Read
   neighbouring files to learn the local style.
3. **One source of truth (§4)** — no duplicated definition where a shared one belongs; contract/schema/
   config changes moved their mirrors, types, and docs in the **same** change.
4. **No fake (§5)** — nothing mocked is presented as real production behavior.
5. **Tests (§2)** — the behavior change is covered by a test that would have failed before it.
6. **Scope** — stays within the spec; flag unrelated drive-by changes.

## How

Start from `git diff`; read surrounding files for context; `grep` to confirm patterns. Cite `file:line`.

## Output

✅ per invariant that holds, ❌ with `file:line` + the fix for each that doesn't. Separate **must-fix**
from **nits**. One-line verdict. Do not modify files.
