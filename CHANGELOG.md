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

### Added

- Default **layered architecture** scaffolded out of the box: `backend/` (Python/FastAPI),
  `frontend/` (React + TS + CSS), `ai/` (agents/prompts/RAG), `infra/` (Terraform) — each with a
  README describing its stack and gates.
- Repository marked as a **GitHub template** — bootstrap with
  `gh repo create … --template …` or the "Use this template" button.

### Changed

- **Simplified placeholders** to project identity only (`{{PROJECT_NAME}}`,
  `{{PROJECT_DESCRIPTION}}`, `{{MAINTAINER}}`, `{{DATE}}`); removed `{{*_DIR}}` and `{{*_CMD}}` now
  that directories and commands are wired to the fixed layers. README documents exactly which file
  holds each placeholder.
- **CI** reworked into one guarded job per layer that auto-skips until the layer has code, so the
  template stays green.
- README bootstrap flow switched from `degit` to a GitHub-native clone/template flow.

## [0.1.0] - 2026-06-22

### Added

- Initial **Dataside-DAIS-Template**: a reusable, spec-first (SDD) + test-first (TDD) project template.
- Project **Constitution** (`.specify/constitution.md`) with seven principles and an amendment process.
- **SDD workflow** under `specs/` with a `_template/` (spec · plan · tasks) and the lifecycle docs.
- **Claude Code** assets — `new-spec`, `grilling`, `verify-gates` skills and `code-reviewer`,
  `test-reviewer`, `docs-i18n-auditor` review agents.
- **Codex parity** — mirrored prompts under `.codex/prompts/` plus the twin `AGENTS.md`.
- **CI workflow** (`.github/workflows/ci.yml`) with Python / Node / frontend quality-gate presets.
- **Release workflow** (`.github/workflows/release.yml`) — pushing a `vX.Y.Z` tag publishes a GitHub
  Release with auto-generated notes.
- Bilingual **README** (English + `README.pt-BR.md`) with diagrams, a table of contents, and a
  Dataside-specific usage guide.
