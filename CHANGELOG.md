# Changelog

All notable changes to this project are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

<!--
Add entries here as you merge changes. When you cut a release, move them under a
new version heading and tag it (`git tag vX.Y.Z && git push origin vX.Y.Z`),
which triggers the Release workflow (.github/workflows/release.yml).

Use these categories: Added · Changed · Deprecated · Removed · Fixed · Security.
-->

### Fixed

- **Release workflow** is now idempotent — re-pushing a tag (or cutting a release by hand) no longer
  fails the job with `Release.tag_name already exists`.

## [0.1.0] - 2026-06-23

First public release — a reusable, spec-first (SDD) + test-first (TDD) project template for
agentic coding (Claude Code + Codex).

### Added

Governance & workflow
- Project **Constitution** (`.specify/constitution.md`) — seven principles + quality gates + amendment process.
- **SDD workflow** under `specs/` with a `_template/` (spec · plan · tasks) and lifecycle docs.
- Worked **example spec** (`specs/001-password-reset/`) and a concrete `docs/architecture.md` — a
  filled model to imitate, marked for deletion when you start your real project.

Architecture
- Default **layered architecture** out of the box: `backend/` (Python/FastAPI), `frontend/`
  (React + TS + CSS), `ai/` (agents/prompts/RAG), `infra/` (Terraform) — each with a README of its
  stack and gates, and "delete if unused" guidance.
- Repository marked as a **GitHub template** — bootstrap with `gh repo create … --template …` or the
  "Use this template" button.

Agent assets
- **Claude Code** — `new-spec`, `grilling`, `verify-gates` skills and `code-reviewer`,
  `test-reviewer`, `docs-i18n-auditor` read-only review subagents.
- **Codex parity** — mirrored prompts under `.codex/prompts/` (`/new-spec`, `/grilling`,
  `/verify-gates`, `/review-code`, `/review-tests`, `/review-docs`) plus the twin `AGENTS.md`.

CI & releases
- **CI workflow** (`.github/workflows/ci.yml`) — one guarded quality-gate job per layer that
  auto-skips until the layer has code, so the template stays green.
- **Release workflow** (`.github/workflows/release.yml`) — pushing a `vX.Y.Z` tag publishes a GitHub
  Release with auto-generated notes.

Open source & developer experience
- **MIT License**.
- `CONTRIBUTING.md`, `SECURITY.md`, and a `.github/PULL_REQUEST_TEMPLATE.md` encoding the
  constitution's "definition of done" (§1–§7).
- Issue templates (bug report · feature request that nudges toward the SDD workflow).
- `.env.example`, `.editorconfig`, and `.github/dependabot.yml`.
- **Bilingual README** (English + `README.pt-BR.md`) with diagrams, a table of contents, a
  "Prerequisites & first run" section, and a usage guide.
