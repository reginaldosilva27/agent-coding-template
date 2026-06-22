# /review-tests

Read-only audit of TDD discipline and test quality (constitution §2). **Audit only — do not edit.**

## Verify

1. **Coverage of intent** — every acceptance criterion in the relevant `specs/NNN-*/spec.md` maps to
   at least one test. Flag any AC with no test.
2. **Behavioral assertions** — tests assert observable behavior, not internal detail, so they survive
   a refactor. Flag tests that pin private internals or pass trivially.
3. **Would have failed first** — the new test actually exercises the new behavior; not a tautology.
4. **Real dependencies (§5)** — where the project uses real external services, the test does too (or is
   marked + skipped when absent). Flag a silent mock standing in for real behavior.
5. **Determinism** — no reliance on wall-clock/order/network flakiness unless that's the point.

## How

Read the spec's ACs, locate the tests that claim to cover them (`grep` the feature name), run the suite
if needed. Cite the AC number + test `file:line`.

## Output

✅ per AC covered, ❌ with the gap + the test that should exist for each that isn't. One-line verdict
(TDD intact / N gaps). Do not modify files.
