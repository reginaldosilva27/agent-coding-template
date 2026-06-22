# Plan: <feature name>

> The HOW. Written after `spec.md` is `clarified`. Decisions here must respect every
> principle in `.specify/constitution.md`; if one must bend, amend the constitution
> first and note it.

## Approach
<!-- The shape of the solution in a few sentences. Alternatives considered + why this one. -->

## Affected files

- `path/to/file` — <what changes>

## Contract / data model changes (constitution §4)
<!-- Public API, schema, config defaults, any cross-cutting contract. Name the single source
     of truth and every place that must move with it (mirrors, docs, types). Migrations? -->

## i18n strings (constitution §7 — only if the project is bilingual)
<!-- Every new user-facing string, in both languages, so nothing ships en-only. Else "n/a". -->

| key / location | en | pt |
|---|---|---|
| | | |

## Test strategy (constitution §2 — TDD)
<!-- Which test files, which level. Each acceptance criterion → at least one test. -->

| Acceptance criterion | Test | File |
|---|---|---|
| AC1 | | |
| AC2 | | |

## Risks / trade-offs
<!-- Perf, backward-compat, assumptions, anything that could bite. -->
