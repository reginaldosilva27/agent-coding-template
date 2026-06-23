# Contributing

Thanks for contributing. Here, **the contribution flow _is_ the SDD + TDD discipline** — the same
rules apply whether you write the code or an AI agent does. The full how-to and the law live in:

- **The how-to:** [`docs/development-workflow.md`](docs/development-workflow.md)
- **The workflow:** [`specs/README.md`](specs/README.md)
- **The law (wins on any conflict):** [`.specify/constitution.md`](.specify/constitution.md)

This page is a thin pointer to those — read them before opening a PR.

## The flow

1. **Fork & branch.** Branch off `main` (e.g. `feat/NNN-feature-name`, `fix/short-description`).
2. **New feature → write a spec first.** Don't jump to code. Copy `specs/_template/` to
   `specs/NNN-feature-name/` and fill `spec.md` (WHAT + WHY + testable acceptance criteria), then
   `plan.md` and `tasks.md`. Use the **`new-spec`** skill / [`/new-spec`](.codex/prompts/new-spec.md)
   Codex prompt, then **clarify** with the **`grilling`** skill (§1). A bug fix or chore needs no spec.
3. **Test-first (TDD).** A failing test before the code — an acceptance criterion for a feature, a
   reproducing case for a bug. Then red → green → refactor. Tests assert behavior, not internals (§2).
4. **Verify the gates.** Run the **`verify-gates`** skill / [`/verify-gates`](.codex/prompts/verify-gates.md)
   prompt before you open the PR — it mirrors CI. "Done" is never declared on red (§3).
5. **Keep agent guidance in sync.** Change a rule in `CLAUDE.md`, change it in `AGENTS.md` and the
   matching `.claude/` ↔ `.codex/` workflow in the **same** commit (§6).
6. **Bilingual user-facing text.** Any string the app renders ships in both English and Portuguese (§7,
   when the project keeps it).

Before the PR, spawn the read-only reviewers for the area you touched: `code-reviewer`, `test-reviewer`,
and — if you touched docs or strings — `docs-i18n-auditor`.

---

## Português 🇧🇷

Obrigado por contribuir. Aqui, **o fluxo de contribuição _é_ a disciplina SDD + TDD** — as mesmas
regras valem se quem escreve o código é você ou um agente de IA. O guia completo e a lei estão em:

- **O guia:** [`docs/development-workflow.md`](docs/development-workflow.md)
- **O fluxo:** [`specs/README.md`](specs/README.md)
- **A lei (vence em qualquer conflito):** [`.specify/constitution.md`](.specify/constitution.md)

Esta página é só um ponteiro para eles — leia antes de abrir um PR.

### O fluxo

1. **Fork & branch.** Crie um branch a partir da `main` (ex.: `feat/NNN-nome`, `fix/descricao-curta`).
2. **Nova feature → escreva a spec primeiro.** Não pule para o código. Copie `specs/_template/` para
   `specs/NNN-nome/` e preencha `spec.md` (O QUÊ + POR QUÊ + critérios de aceite testáveis), depois
   `plan.md` e `tasks.md`. Use a skill **`new-spec`** / o prompt [`/new-spec`](.codex/prompts/new-spec.md)
   do Codex, e então **clarifique** com a skill **`grilling`** (§1). Bug fix ou tarefa simples não
   precisa de spec.
3. **Teste primeiro (TDD).** Um teste que falha antes do código — um critério de aceite para uma
   feature, um caso que reproduz o bug. Então red → green → refactor. Testes verificam comportamento,
   não detalhes internos (§2).
4. **Verifique os gates.** Rode a skill **`verify-gates`** / o prompt
   [`/verify-gates`](.codex/prompts/verify-gates.md) antes de abrir o PR — ele espelha o CI. "Pronto"
   nunca é declarado no vermelho (§3).
5. **Mantenha a orientação dos agentes em sincronia.** Mudou uma regra no `CLAUDE.md`, mude no
   `AGENTS.md` e no workflow correspondente `.claude/` ↔ `.codex/` no **mesmo** commit (§6).
6. **Texto bilíngue para o usuário.** Qualquer string que o app exibe vai em inglês e português (§7,
   quando o projeto mantém esse princípio).

Antes do PR, acione os revisores read-only da área que você tocou: `code-reviewer`, `test-reviewer` e
— se mexeu em docs ou strings — `docs-i18n-auditor`.
