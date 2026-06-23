# Tasks: Password reset via email link

> 📦 **EXAMPLE shipped with the template** — see the callout in `spec.md`. Delete this folder
> when you start your real project. All boxes are checked to show what a *completed* feature
> looks like.

> The work, ordered, as a TDD checklist. Each implementation task is preceded by the
> test that should fail first (red → green → refactor). Check boxes as you go.

## Tasks

- [x] **T0 — scaffold**: add the `password_reset_token` model + migration and the
  `PASSWORD_RESET_TTL_MINUTES = 30` config constant (no behavior yet).
- [x] **T1 — test first (AC1)**: write failing `test_request_issues_single_token_and_sends_email`.
- [x] **T2 — implement**: `POST /auth/password-reset/request` issues one token + calls the email sender → T1 green.
- [x] **T3 — test first (AC2)**: write failing `test_unknown_email_same_response_no_email_sent`.
- [x] **T4 — implement**: make the unknown-email branch byte-identical and send no email → T3 green.
- [x] **T5 — test first (AC8)**: write failing `test_stored_token_is_hashed_not_raw`.
- [x] **T6 — implement**: persist only the token hash; return the raw token to the sender → T5 green.
- [x] **T7 — test first (AC3)**: write failing `test_reset_link_expires`.
- [x] **T8 — implement**: `confirm` rejects tokens past `expires_at` (`410 Gone`) → T7 green.
- [x] **T9 — test first (AC4)**: write failing `test_consumed_token_is_rejected`.
- [x] **T10 — implement**: mark token consumed on success; reject already-consumed tokens → T9 green.
- [x] **T11 — test first (AC5)**: write failing `test_new_request_invalidates_previous_token`.
- [x] **T12 — implement**: a new request invalidates the prior token for that account → T11 green.
- [x] **T13 — test first (AC6)**: write failing `test_weak_password_rejected_password_unchanged`.
- [x] **T14 — implement**: run the shared password-policy validator before applying → T13 green.
- [x] **T15 — test first (AC7)**: write failing `test_successful_reset_replaces_password_and_consumes_token`.
- [x] **T16 — implement**: replace the credential hash + consume the token in one transaction → T15 green.
- [x] **T17 — frontend**: build the Forgot / Reset screens; add `renders confirm form and surfaces policy error`.
- [x] **T18 — contract/docs (§4)**: regenerate the OpenAPI schema; update `docs/architecture.md` in the same change.
- [x] **T19 — i18n (§7)**: add the en + pt strings from `plan.md` to `frontend/src/i18n/strings.ts`.
- [x] **T20 — refactor**: extract constant-time token compare; remove duplication; keep all tests green.

## Definition of done

- [x] Every acceptance criterion in `spec.md` maps to a passing test
- [x] Lint clean
- [x] Tests green
- [x] Type-check / build passes
- [x] Docs updated in the same change; cross-cutting contract in sync (§4)
- [x] (If bilingual) all new user-facing text exists in en **and** pt
- [x] `spec.md` status updated to `done`
