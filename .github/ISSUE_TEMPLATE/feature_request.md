---
name: ✨ Feature request
about: Propose new behavior. A feature starts with a spec (SDD §1) — this issue is its raw material.
title: "[feature]: "
labels: enhancement
---

> 🧩 **This project is spec-first.** A new feature does **not** start with code — it starts with a
> spec under `specs/NNN-feature-name/`, scaffolded via the **`/new-spec`** skill (or
> `.codex/prompts/new-spec.md`). The fields below are the raw material that becomes that spec.
> _Este projeto é spec-first: toda feature começa por uma spec, não por código._

## 🎯 What / O quê

<!-- What should the system do? Describe the behavior, not the implementation. -->
<!-- O que o sistema deve fazer? Descreva o comportamento, não a implementação. -->

## 💡 Why / Por quê

<!-- The problem this solves and who benefits. / O problema que resolve e quem se beneficia. -->

## ✅ Acceptance criteria / Critérios de aceitação

<!-- Numbered, testable statements — each becomes a failing test first, then code (§2). -->
<!-- Afirmações numeradas e testáveis — cada uma vira um teste que falha antes do código. -->

1.
2.
3.

## 🧭 Scope & context / Escopo e contexto

- Layer(s) affected / Camada(s) afetada(s) (`backend` · `frontend` · `ai` · `infra`):
- User-facing text? → needs en **and** pt (§7) / Texto para o usuário? → precisa de en **e** pt:
- Open questions to clarify (the `/grilling` step) / Dúvidas a esclarecer:

<!--
Next step: run `/new-spec` to scaffold specs/NNN-…, then `/grilling` to resolve open questions
before any code (constitution §1).
-->
