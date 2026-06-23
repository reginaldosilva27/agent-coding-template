# Tasks: <feature name>

> The work, ordered, as a TDD checklist. Each implementation task is preceded by the
> test that should fail first (red → green → refactor). Check boxes as you go.

## Tasks

- [ ] **T1 — <test first>**: write failing test for AC1
- [ ] **T2 — <implement>**: make T1 pass
- [ ] **T3 — <test first>**: write failing test for AC2
- [ ] **T4 — <implement>**: make T3 pass
- [ ] **T5 — contract/docs**: update the single source of truth + docs in the same change (§4)
- [ ] **T6 — i18n**: add en + pt strings (constitution §7, if applicable)
- [ ] **T7 — refactor**: clean up, keep tests green

## Definition of done

- [ ] Every acceptance criterion in `spec.md` maps to a passing test
- [ ] Lint clean
- [ ] Tests green
- [ ] Type-check / build passes
- [ ] Docs updated in the same change; cross-cutting contract in sync (§4)
- [ ] (If bilingual) all new user-facing text exists in en **and** pt
- [ ] `spec.md` status updated to `done`
