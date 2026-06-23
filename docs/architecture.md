# Architecture

> 📦 **Illustrative example — replace per project.** This file documents the *example*
> password-reset system described in [`specs/001-password-reset/`](../specs/001-password-reset/),
> so you can see what a real, non-stub `architecture.md` looks like. **Replace its contents** with
> the architecture of *your* system once it exists. This is the living description of the
> **current** system (the specs are the decision history; this is the state). Keep it in sync with
> the code in the same change that alters behavior (constitution §4).

## Overview

Self-service password reset over email. A user proves control of their registered inbox by
following a one-time link, then sets a new password. The flow is two stateless HTTP endpoints on
the **backend** layer, two screens on the **frontend** layer, and a single token store in the
database. The defining constraint is that the system never reveals whether an email is registered:
the request endpoint behaves identically for known and unknown emails.

## Components

| Component | Layer | Responsibility |
|---|---|---|
| `POST /auth/password-reset/request` | `backend/` | Look up the email; if registered, issue a token and send the link. Always returns a neutral `202`. |
| `POST /auth/password-reset/confirm` | `backend/` | Validate the token (exists, not expired, not consumed), enforce the password policy, replace the credential, consume the token. |
| `password_reset.py` service | `backend/` | Token issue/verify logic; the only place that hashes and compares tokens. |
| `password_reset_token` store | DB | One row per outstanding token: account id, **token hash**, `expires_at`, `consumed_at`. |
| Email sender | injected dependency | Delivers the link. An interface, not a concrete provider — faked (and *labelled* as a double) in tests, real in deployed environments (§5). |
| Forgot / Reset screens | `frontend/` | The "enter your email" and "set a new password" forms, plus the neutral confirmation and the invalid-link message. |

```
[Forgot screen] ──request(email)──▶ /request ──▶ issue token ──hash──▶ [token store]
                                                        │
                                                  raw token only
                                                        ▼
                                                  [email sender] ──link──▶ user inbox
user clicks link
      │ raw token
      ▼
[Reset screen] ──confirm(token,new_pw)──▶ /confirm ──hash+lookup──▶ [token store]
                                                  │ valid?
                                                  ▼
                                  enforce policy ─▶ replace credential ─▶ consume token
```

## Contracts & sources of truth

- **HTTP contract** — the OpenAPI schema generated from the route definitions in
  `backend/app/auth/routes.py` is the single source of truth for request/response shapes and status
  codes (`202` request, `204` success, `400` policy violation, `410 Gone` invalid/expired/used
  token). This doc links to it rather than restating field lists, so the two cannot drift (§4).
- **Token lifetime** — `PASSWORD_RESET_TTL_MINUTES` in `backend/app/config.py` is the *one*
  definition of the 30-minute expiry. Both the issuer and the verifier read it; there is no other
  literal `30` in the flow.
- **Token record shape** — the `password_reset_token` model is the source of truth; the SQL
  migration mirrors it. Changing the model means changing the migration in the same commit.
- **Password policy** — the existing signup validator (`password_policy.py`) is reused unchanged;
  reset does not get its own copy of the rule (§4).
- **User-facing strings** — `frontend/src/i18n/strings.ts` holds every reset string in `{ en, pt }`
  (§7). No screen hard-codes prose.

## Data / token flow (the security-critical path)

1. **Request.** Email is looked up. *Registered:* a high-entropy random token is generated, its
   **hash** is stored with `expires_at = now + TTL`, any prior token for the account is
   invalidated, and the **raw** token is handed to the email sender. *Not registered:* nothing is
   stored and **no email is sent**. Both branches return the identical neutral `202`.
2. **Confirm.** The presented raw token is hashed and looked up via a **constant-time** comparison.
   It is rejected (`410 Gone`) if missing, if `now > expires_at`, or if `consumed_at` is set. On a
   valid token the new password is checked against the shared policy, the credential hash is
   replaced, and `consumed_at` is set — all in one transaction.

## Non-obvious invariants

These are the things a contributor could break without noticing — reviewers guard them:

- **No enumeration.** The request endpoint's response and observable timing must be identical for
  known and unknown emails, and email sending happens *only* on the registered branch. A change
  that returns a different status, body, or noticeably different latency for unknown emails breaks
  AC2 even if every test about *successful* reset still passes.
- **Raw tokens are never persisted or logged.** Only the hash is stored (AC8). The raw token lives
  exactly once, in the email link. Logging it, returning it in a response, or storing it defeats
  the whole design.
- **Single-use and single-active.** A token works at most once (`consumed_at`), and at most one
  token per account is ever valid (a new request invalidates the previous). Both rely on the token
  store, not on the token being a stateless JWT — that's why a DB row exists.
- **Constant-time comparison.** Token-hash comparison must not short-circuit, or validity leaks
  through timing.
- **Reset does not create a session.** A successful reset returns the user to login; it does not
  authenticate them.
