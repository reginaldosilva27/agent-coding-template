# Spec: Password reset via email link

> 📦 **EXAMPLE shipped with the template.** This `specs/001-password-reset/` folder is a
> fully-worked model so you can see what a real, finished spec looks like end to end
> (`spec → plan → tasks`, every acceptance criterion mapped to a test). **Delete this whole
> folder when you start your real project** — keep `specs/_template/` and `specs/README.md`.
> The code it describes is illustrative; it is not actually implemented in this template repo.

| | |
|---|---|
| **ID** | 001-password-reset |
| **Status** | ~~draft~~ → ~~clarified~~ → ~~planned~~ → ~~in-progress~~ → **done** |
| **Author** | Reginaldo Silva |
| **Date** | 2026-06-23 |

> Fill the WHAT and the WHY. **No implementation detail here** — that belongs in
> `plan.md`. If you catch yourself naming a file or a function, move it to the plan.

## Problem / motivation

Users who forget their password have no self-service way back into their account; today the
only path is a manual support request, which is slow for the user and a load on the team. We
need a secure, self-service flow that lets a user prove control of their registered email and
set a new password — without ever leaking whether an email is registered, and without leaving a
reusable credential lying around.

## Goals

- A user can request a reset for their email and, if it exists, receive a single-use link.
- The link proves possession of the inbox and lets the user set a new password.
- Tokens are short-lived, single-use, and tied to exactly one account.
- The flow never reveals whether a given email is registered (no account enumeration).

## Non-goals

- Multi-factor / TOTP recovery, SMS reset, or security-question fallback.
- Rate-limiting / abuse throttling (tracked separately — see *Out of scope*).
- The email transport itself (templating, deliverability, provider choice) — we only require
  that "an email containing the link is sent". The provider is an injected dependency.
- Session/login mechanics. Resetting a password does not by itself log the user in.

## User-facing behavior

1. From the login screen the user clicks **"Forgot password?"**, enters their email, and submits.
2. They always see the same neutral confirmation ("If that email is registered, we've sent a
   reset link") — regardless of whether the account exists.
3. If the account exists, an email arrives with a link containing a one-time token.
4. Following the link shows a "set a new password" form. On success they see a confirmation and
   can log in with the new password. An expired, already-used, or unknown token shows a clear
   "this link is no longer valid — request a new one" message.

Bilingual strings (constitution §7) are listed in `plan.md`.

## Acceptance criteria
<!-- Numbered, each one TESTABLE. These become the failing tests in tasks.md (TDD). -->

1. **AC1 — Request emits a one-time token.** Given a registered email, when a reset is
   requested, then exactly one reset token is generated for that account and an email
   containing the link is sent.
2. **AC2 — No account enumeration.** Given an email that is *not* registered, when a reset is
   requested, then the response is byte-for-byte identical to the registered case and **no**
   email is sent.
3. **AC3 — Token expires after 30 minutes.** Given a valid token, when it is presented more than
   30 minutes after issuance, then the reset is rejected as expired and the password is unchanged.
4. **AC4 — A used token is rejected.** Given a token that has already completed a successful
   reset, when it is presented again, then the second attempt is rejected and the password is
   unchanged.
5. **AC5 — One active token per account.** Given an account with an outstanding token, when a
   new reset is requested, then the previous token is invalidated and only the newest token works.
6. **AC6 — Password rule enforced.** Given a valid token, when the user submits a new password
   that violates the policy (min 8 chars, at least one letter and one digit), then the reset is
   rejected with a validation error and the password is unchanged.
7. **AC7 — Successful reset replaces the credential.** Given a valid token and a policy-compliant
   password, when the reset is submitted, then the stored password hash is replaced, the token is
   consumed, and the old password no longer authenticates.
8. **AC8 — Tokens are not stored in the clear.** Given an issued token, when persistence is
   inspected, then only a hash of the token is stored — the raw token exists only in the email link.

## Contract / data impact
<!-- Does this change a public API, a schema, a config default, or any cross-cutting contract? -->

- **New public endpoints** (source of truth: the OpenAPI schema generated from the backend route
  definitions):
  - `POST /auth/password-reset/request` — body `{ email }` → always `202 Accepted`, neutral body.
  - `POST /auth/password-reset/confirm` — body `{ token, new_password }` → `204` on success,
    `400` on policy violation, `410 Gone` on expired/used/unknown token.
- **New persistence**: a `password_reset_token` record (account id, token *hash*, `expires_at`,
  `consumed_at`). Single source of truth for the token lifetime constant (30 min) is one config
  value referenced by both the issuing and verifying code (constitution §4).
- **No change** to the existing user/credential schema beyond the password hash being updated
  through the existing credential-storage path.

## Open questions (clarify before planning)
<!-- The "grill me" list. Emptied during the clarify step. -->

- [x] Should the reset link expire? → **Yes, 30 minutes.** (clarify)
- [x] One active reset token per user, or many? → **One** — a new request invalidates the prior.
- [x] What password policy applies? → Reuse the existing signup policy: **≥ 8 chars, ≥ 1 letter,
  ≥ 1 digit.**
- [x] Does a successful reset also log the user in? → **No** — they return to the login screen.

## Out of scope / deferred
<!-- Ideas raised but parked. -->

- Rate-limiting reset requests per email / per IP (abuse protection) — separate spec.
- Notifying the user by email that their password *was* changed.
- Invalidating active sessions on the account after a reset.
