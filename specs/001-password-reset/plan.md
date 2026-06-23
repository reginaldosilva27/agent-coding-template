# Plan: Password reset via email link

> 📦 **EXAMPLE shipped with the template** — see the callout in `spec.md`. Delete this folder
> when you start your real project.

> The HOW. Written after `spec.md` is `clarified`. Decisions here must respect every
> principle in `.specify/constitution.md`; if one must bend, amend the constitution
> first and note it.

## Approach

Two stateless endpoints over a single new token store. **Request** looks up the account; if it
exists it generates a high-entropy random token, stores only its hash with a 30-minute expiry,
invalidates any prior token for that account, and hands the raw token to the email sender. The
response is the same neutral `202` whether or not the account exists, and the email send happens
only on the existing-account branch — this is what makes AC2 (no enumeration) hold.

**Confirm** hashes the presented token, looks it up, and rejects it if missing, expired, or
already consumed (constant-time hash comparison to avoid leaking validity through timing). On a
valid token it runs the shared password-policy validator, replaces the credential via the
existing hashing path, and marks the token consumed in the same transaction.

Alternatives considered: a stateless signed JWT as the token (no DB row) — rejected because it
can't satisfy single-use (AC4) or one-active-token (AC5) without a server-side record anyway, so
the DB row is unavoidable and simpler to reason about.

## Affected files

- `backend/app/auth/password_reset.py` — **new**: request/confirm service logic, token issue/verify.
- `backend/app/auth/routes.py` — add the two routes; wire the email-sender dependency.
- `backend/app/auth/password_policy.py` — **reuse** the existing signup validator (no change).
- `backend/app/models/password_reset_token.py` — **new**: the token record (account id, token hash,
  `expires_at`, `consumed_at`).
- `backend/app/config.py` — add `PASSWORD_RESET_TTL_MINUTES = 30` (single source of truth for AC3).
- `backend/migrations/00X_password_reset_token.sql` — **new**: create the token table.
- `frontend/src/pages/ForgotPassword.tsx` + `ResetPassword.tsx` — **new**: the two screens.
- `frontend/src/i18n/strings.ts` — add the en/pt strings below.
- `docs/architecture.md` — document the token flow + invariants in the same change (§4).
- `backend/tests/auth/test_password_reset.py`, `frontend/src/pages/__tests__/reset.test.tsx` — tests.

## Contract / data model changes (constitution §4)

- **OpenAPI schema** is the single source of truth for the two endpoints; it is generated from the
  route definitions in `routes.py`, so the route signatures and the published schema cannot drift.
  `docs/architecture.md` links to it rather than restating it.
- **`PASSWORD_RESET_TTL_MINUTES`** in `config.py` is the *one* definition of the 30-minute lifetime;
  both the issuer and the verifier read it. No magic `30` anywhere else.
- **Token-hash storage**: the raw token is never persisted (AC8). The migration introduces
  `password_reset_token`; the model is the source of truth for its shape and the migration mirrors it.

## i18n strings (constitution §7 — the project is bilingual)

| key / location | en | pt |
|---|---|---|
| `auth.reset.request.cta` | Forgot password? | Esqueceu a senha? |
| `auth.reset.request.confirm` | If that email is registered, we've sent a reset link. | Se esse e-mail estiver cadastrado, enviamos um link de redefinição. |
| `auth.reset.confirm.title` | Set a new password | Defina uma nova senha |
| `auth.reset.confirm.success` | Your password has been updated. You can now log in. | Sua senha foi atualizada. Você já pode entrar. |
| `auth.reset.error.invalid` | This link is no longer valid — request a new one. | Este link não é mais válido — solicite um novo. |
| `auth.reset.error.policy` | Password must be at least 8 characters and include a letter and a digit. | A senha deve ter pelo menos 8 caracteres e incluir uma letra e um número. |

## Test strategy (constitution §2 — TDD)

Each acceptance criterion maps to at least one behavioral test. Backend tests use the real token
store (a test DB) and a captured fake email sender that records sends without fabricating delivery
(constitution §5 — the fake is clearly a test double, not presented as a real provider).

| Acceptance criterion | Test | File |
|---|---|---|
| AC1 — request emits one-time token | `test_request_issues_single_token_and_sends_email` | `backend/tests/auth/test_password_reset.py` |
| AC2 — no account enumeration | `test_unknown_email_same_response_no_email_sent` | `backend/tests/auth/test_password_reset.py` |
| AC3 — token expires after 30 min | `test_reset_link_expires` | `backend/tests/auth/test_password_reset.py` |
| AC4 — used token rejected | `test_consumed_token_is_rejected` | `backend/tests/auth/test_password_reset.py` |
| AC5 — one active token per account | `test_new_request_invalidates_previous_token` | `backend/tests/auth/test_password_reset.py` |
| AC6 — password rule enforced | `test_weak_password_rejected_password_unchanged` | `backend/tests/auth/test_password_reset.py` |
| AC7 — successful reset replaces credential | `test_successful_reset_replaces_password_and_consumes_token` | `backend/tests/auth/test_password_reset.py` |
| AC8 — tokens not stored in clear | `test_stored_token_is_hashed_not_raw` | `backend/tests/auth/test_password_reset.py` |
| AC1/AC6 (UI) | `renders confirm form and surfaces policy error` | `frontend/src/pages/__tests__/reset.test.tsx` |

## Risks / trade-offs

- **Timing side-channel.** Verification must compare token hashes in constant time and keep the
  request path indistinguishable for known vs. unknown emails, or AC2 leaks via timing. Flagged
  for the `code-reviewer`.
- **Clock skew** on expiry (AC3) is evaluated server-side against `expires_at`; no client clock trust.
- **Email transport is mocked in tests** by design (a recording double) — honest because it is
  clearly a test double and the real sender is an injected dependency exercised in higher-level
  environments, not faked-as-real in committed code (§5).
- Deferring rate-limiting means the request endpoint is open to email-spamming; acceptable for
  this iteration, tracked as a follow-up spec.
