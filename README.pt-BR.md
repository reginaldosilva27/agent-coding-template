<div align="center">

# 🧭 Dataside-DAIS-Template

### Um ponto de partida reutilizável que dá a todo projeto a mesma disciplina de engenharia

**Desenvolvimento Orientado a Especificação (SDD)** · **Desenvolvimento Orientado a Testes (TDD)** ·
uma **Constituição** do projeto · skills e subagents de revisão prontos no **Claude Code** ·
**Releases** e **Changelog** automatizados

<br />

![SDD](https://img.shields.io/badge/Orientado_a_Spec-obrigat%C3%B3rio-2563eb?style=for-the-badge)
![TDD](https://img.shields.io/badge/Orientado_a_Teste-obrigat%C3%B3rio-16a34a?style=for-the-badge)
![Claude Code](https://img.shields.io/badge/Claude_Code-pronto-d97706?style=for-the-badge)

<br />

**🌐 Idioma:** [English](./README.md) · **Português** 🇧🇷

</div>

---

## 📑 Sumário

1. [Introdução](#-1-introdução)
2. [Arquitetura do Projeto](#-2-arquitetura-do-projeto)
3. [Conceitos & Filosofia](#-3-conceitos--filosofia)
4. [Skills & Subagents no Claude Code](#-4-skills--subagents-no-claude-code)
5. [Fluxo de CI e Workflows](#-5-fluxo-de-ci-e-workflows)
6. [Releases e Changelog](#-6-releases-e-changelog)
7. [Usando em um Projeto Real da Dataside](#-7-usando-em-um-projeto-real-da-dataside)
8. [Roadmap — Padrões Dataside](#-8-roadmap--padrões-dataside)

---

## 🚀 1. Introdução

Este repositório é um **template**: você não o executa, você **começa a partir dele**. Todo projeto
que nasce aqui herda a mesma forma opinativa e inegociável de construir software — de modo que o 10º
projeto do time se pareça e se comporte como o 1º, e qualquer pessoa de engenharia (ou agente de IA)
consiga entrar e já saber as regras.

O que você ganha sem configurar nada:

| 🎁 | Recurso |
|----|---------|
| 📜 | Uma **Constituição** — os princípios que toda mudança deve respeitar, com processo de emenda. |
| 🧩 | **SDD** — escreva o *o quê* e o *porquê* antes do código, revisados primeiro. |
| 🧪 | **TDD** — critérios de aceite viram testes que falham antes da implementação. |
| 🤖 | **Skills** e **subagents** de revisão prontos para o Claude Code. |
| ⚙️ | **Workflows de CI** — quality gates multi-stack (presets Python / Node / frontend). |
| 🏷️ | **Releases automatizados** — uma tag `vX.Y.Z` → uma GitHub Release com notas automáticas. |

> **A filosofia em uma linha:** *Nenhum código de feature sem uma spec. Nenhuma mudança de
> comportamento sem um teste que falha primeiro. Nada é "pronto" até os gates ficarem verdes.*

### 🗺️ A jornada de uma nova feature

Toda feature percorre o mesmo caminho — da ideia ao PR mergeado. As skills do Claude Code (em
**laranja**) e os subagents de revisão (em **roxo**) se encaixam em cada etapa:

```mermaid
flowchart TD
    Start(["💡 Ideia de nova feature"]) --> Q{"Precisa de spec?<br/>novo comportamento ou<br/>contrato público?"}

    Q -->|"Sim — feature"| Spec["🆕 <b>/new-spec</b><br/>cria specs/NNN/<br/>O QUÊ + PORQUÊ"]
    Q -->|"Não — bug / chore"| Red

    Spec --> Clarify["🔍 <b>/grilling</b><br/>clarifica dúvidas,<br/>uma de cada vez"]
    Clarify --> Plan["🗺️ plan.md → tasks.md<br/>COMO + checklist TDD"]
    Plan --> Red

    Red["🔴 Escreve um teste que falha"] --> Green["🟢 Faz passar"]
    Green --> Refactor["♻️ Refatora"]
    Refactor --> More{"Mais tasks?"}
    More -->|"Sim"| Red
    More -->|"Não"| Verify["🚦 <b>/verify-gates</b><br/>lint · testes · build"]

    Verify --> Review["🔎 aciona os subagents de revisão<br/><b>code-reviewer</b> · <b>test-reviewer</b>"]
    Review --> PR(["✅ Abre o PR"])

    style Start fill:#1e293b,color:#fff
    style Q fill:#0f172a,color:#fff
    style More fill:#0f172a,color:#fff
    style Spec fill:#d97706,color:#fff
    style Clarify fill:#d97706,color:#fff
    style Verify fill:#d97706,color:#fff
    style Review fill:#7c3aed,color:#fff
    style Red fill:#dc2626,color:#fff
    style Green fill:#16a34a,color:#fff
    style Refactor fill:#0d9488,color:#fff
    style PR fill:#16a34a,color:#fff
```

> Um passo a passo real desse fluxo dentro do Claude Code está em
> [§4 — Skills & Subagents](#-4-skills--subagents-no-claude-code).

---

## 🧱 2. Arquitetura do Projeto

O template é um conjunto de **portas de entrada em camadas sobre um único conjunto de regras.** A
Constituição é a lei; todo o resto é uma forma de aplicá-la ou de fazê-la valer.

```mermaid
flowchart TD
    C["📜 .specify/constitution.md<br/><b>A Lei — fonte única da verdade</b>"]

    C --> G["📋 Documentos de governança"]
    C --> A["🤖 Porta do Claude Code"]
    C --> E["⚙️ Aplicação automatizada"]

    G --> G1["specs/README.md<br/><i>fluxo SDD</i>"]
    G --> G2["docs/development-workflow.md<br/><i>guia do contribuidor</i>"]
    G --> G3["CLAUDE.md<br/><i>guia sempre ativo do agente</i>"]

    A --> A1[".claude/skills/<br/><i>executam os rituais</i>"]
    A --> A2[".claude/agents/<br/><i>revisores somente leitura</i>"]

    E --> E1[".github/workflows/ci.yml<br/><i>quality gates</i>"]
    E --> E2[".github/workflows/release.yml<br/><i>tag → release</i>"]

    style C fill:#2563eb,color:#fff,stroke:#1e40af,stroke-width:2px
    style G fill:#1e293b,color:#fff
    style A fill:#1e293b,color:#fff
    style E fill:#1e293b,color:#fff
```

### As etapas de desenvolvimento (o pipeline conceitual)

```mermaid
flowchart LR
    S["📝 <b>specify</b><br/>spec.md<br/>O QUÊ + PORQUÊ"]
    CL["🔍 <b>clarify</b><br/>questionar as<br/>dúvidas"]
    P["🗺️ <b>plan</b><br/>plan.md<br/>COMO"]
    T["✅ <b>tasks</b><br/>tasks.md<br/>checklist TDD"]
    I["🔴🟢♻️ <b>implement</b><br/>red → green<br/>→ refactor"]
    V["🚦 <b>verify</b><br/>gates<br/>verdes"]

    S --> CL --> P --> T --> I --> V

    style S fill:#2563eb,color:#fff
    style CL fill:#7c3aed,color:#fff
    style P fill:#0891b2,color:#fff
    style T fill:#0d9488,color:#fff
    style I fill:#16a34a,color:#fff
    style V fill:#d97706,color:#fff
```

### Mapa do repositório

```
📦 Dataside-DAIS-Template
├── 📜 .specify/
│   └── constitution.md          # os princípios inegociáveis (edite por projeto)
├── 📂 specs/
│   ├── README.md                # o fluxo SDD
│   └── _template/               # copie isto para iniciar uma feature
│       ├── spec.md              #   O QUÊ + PORQUÊ + critérios de aceite
│       ├── plan.md              #   COMO — abordagem, arquivos afetados
│       └── tasks.md             #   o trabalho, como checklist TDD
├── 🤖 .claude/
│   ├── README.md                # índice de skills & subagents
│   ├── skills/                  # new-spec · grilling · verify-gates
│   └── agents/                  # code-reviewer · test-reviewer · docs-i18n-auditor
├── ⚙️  .github/workflows/
│   ├── ci.yml                   # quality gates multi-stack
│   └── release.yml              # tag vX.Y.Z → GitHub Release
├── 📖 docs/
│   ├── architecture.md          # descrição viva do sistema em execução
│   └── development-workflow.md  # o companheiro do contribuidor para a constituição
├── 🧭 CLAUDE.md                  # guia sempre ativo para o Claude Code
└── 🏷️  CHANGELOG.md              # esqueleto Keep a Changelog
```

> 🔁 **Usando OpenAI Codex em vez do Claude Code?** Os mesmos padrões estão espelhados em `AGENTS.md`
> e `.codex/` — deixados de fora deste guia por enquanto para focar em uma ferramenta só.

---

## 💡 3. Conceitos & Filosofia

Três ideias sustentam o template inteiro.

### 📜 A Constituição é a fonte única da verdade

[`​.specify/constitution.md`](.specify/constitution.md) guarda os princípios. Ela é emendada **de
propósito** (em um PR, com justificativa) — nunca por acidente. Todo o resto — os docs, o guia dos
agentes, o CI — *deriva dela*. Quando duas coisas conflitam, a Constituição vence.

<details>
<summary><b>Os sete princípios (clique para expandir)</b></summary>

| § | Princípio | Em resumo |
|---|-----------|-----------|
| **§1** | Spec primeiro (SDD) | Nenhum código de feature sem uma spec em `specs/`. |
| **§2** | Teste primeiro (TDD) | Um teste que falha antes da implementação; testes verificam comportamento. |
| **§3** | Pronto = gates verdes | Lint, testes, build passam. "Pronto" nunca é declarado no vermelho. |
| **§4** | Fonte única da verdade | Cada fato vive em um lugar; docs acompanham o código na mesma mudança. |
| **§5** | Honestidade | Nada falso é apresentado como real; má configuração falha rápido. |
| **§6** | Guia dos agentes em sincronia | `CLAUDE.md` ↔ `AGENTS.md` ↔ `.claude`/`.codex` andam juntos. |
| **§7** | *(Opcional)* Bilíngue | Texto voltado ao usuário é entregue em `en` e `pt`. |

</details>

### 🧩 SDD — escreva a intenção antes do código

A spec responde **o quê** e **porquê**; o plano responde **como**; as tasks são **o trabalho**. Specs
são um **registro de decisões append-only** (como ADRs/RFCs) — mantidas para sempre, nunca apagadas
nem renumeradas. O *estado atual* do sistema vive em `docs/`, o *histórico de decisões* vive em
`specs/`.

### 🧪 TDD — prove que funciona antes de confiar

Cada critério de aceite numa spec é uma *afirmação testável* que vira um *teste*. Ciclo:
**🔴 red** (escreva o teste que falha) → **🟢 green** (faça passar) → **♻️ refactor**. Uma feature
finalizada consegue apontar de cada critério para o teste que o comprova.

---

## 🤖 4. Skills & Subagents no Claude Code

O Claude Code lê [`CLAUDE.md`](CLAUDE.md) automaticamente — é o livro de regras sempre ativo. A pasta
[`.claude/`](.claude/) então adiciona dois tipos de ajudantes com **funções diferentes**:

```mermaid
flowchart TB
    subgraph SK["🧩 SKILLS — invoque para <b>FAZER</b> um ritual"]
        direction LR
        S1["/new-spec<br/><i>cria a spec</i>"]
        S2["/grilling<br/><i>clarifica por entrevista</i>"]
        S3["/verify-gates<br/><i>roda os gates</i>"]
    end

    subgraph AG["🔎 SUBAGENTS — acione para <b>CHECAR</b> (somente leitura)"]
        direction LR
        A1["code-reviewer<br/><i>correção + escopo</i>"]
        A2["test-reviewer<br/><i>disciplina TDD</i>"]
        A3["docs-i18n-auditor<br/><i>docs + i18n</i>"]
    end

    SK -->|"construa usando estas"| AG

    style SK fill:#0d9488,color:#fff
    style AG fill:#7c3aed,color:#fff
```

- **🧩 Skills** codificam um ritual multi-arquivo para você não fazê-lo na mão. Você as invoca com um
  comando de barra (`/new-spec`) ou simplesmente pedindo. Elas **editam arquivos e conduzem o fluxo**.
- **🔎 Subagents** são revisores somente leitura. Eles **reportam, não editam** — acione-os antes de um
  PR para a área que você mexeu.

| Ajudante | Tipo | O que faz |
|----------|------|-----------|
| `/new-spec` | 🧩 skill | Cria `specs/NNN-*/` a partir do template — **antes de qualquer código** (§1). |
| `/grilling` | 🧩 skill | O motor de **clarify** — entrevista uma pergunta por vez para eliminar ambiguidade. |
| `/verify-gates` | 🧩 skill | Roda o espelho local do CI + os gates transversais da constituição. |
| `code-reviewer` | 🔎 subagent | Correção, convenções, fonte única da verdade, escopo. |
| `test-reviewer` | 🔎 subagent | Cada critério de aceite mapeado para um teste de comportamento. |
| `docs-i18n-auditor` | 🔎 subagent | Docs acompanham o código, paridade `en`/`pt` (se bilíngue). |

### 🎬 Uma sessão real — construindo "redefinição de senha"

Veja como o fluxo acima realmente aparece numa conversa com o Claude Code:

```text
Você   ▶  /new-spec redefinição de senha por link de e-mail
Claude ▶  📁 Criei specs/003-redefinicao-senha/ (spec.md, plan.md, tasks.md)
          Rascunhei spec.md com 4 critérios de aceite. Restam algumas dúvidas em aberto.

Você   ▶  /grilling
Claude ▶  P1: O link de redefinição deve expirar? (Recomendado: sim, 30 min) ─ sua resposta?
Você   ▶  sim, 30 minutos
Claude ▶  P2: Um token de redefinição ativo por usuário, ou vários? (Recomendado: um) ─ ?
          …dobra cada resposta de volta no spec.md, move para "clarified".

Você   ▶  planeje e implemente task por task
Claude ▶  🔴 AC-1 → escreve o teste que falha test_link_redefinicao_expira() …
          🟢 implementa até passar …
          ♻️ refatora. Marca o box. Vai para o AC-2.

Você   ▶  /verify-gates
Claude ▶  ✅ lint · ✅ 12 testes · ✅ build — tudo verde.

Você   ▶  use os subagents code-reviewer e test-reviewer nas minhas mudanças
Claude ▶  🔎 code-reviewer: 1 achado (comparação de token não é constant-time) …
          🔎 test-reviewer: cada AC mapeia para um teste ✓. Nenhum achado.
```

> 💡 **Duas formas de acionar um subagent:** peça em linguagem natural (*"revise minhas mudanças com o
> code-reviewer"*) ou deixe o Claude acioná-lo automaticamente na etapa de revisão. De qualquer forma
> ele só **lê** — você mantém o controle de cada edição.

> ➕ **Estenda:** adicione skills específicas do projeto (`add-endpoint`, `add-db-table`, …) em
> `.claude/skills/` para codificar os rituais multi-arquivo recorrentes do seu código.

---

## 🔄 5. Fluxo de CI e Workflows

Os quality gates da Constituição (§3) são aplicados por [`.github/workflows/ci.yml`](.github/workflows/ci.yml).
Ele já vem com **presets** para Python, Node e frontend — mantenha o que usa, apague o resto.

```mermaid
flowchart LR
    PR["📤 push na main<br/>/ abrir PR"] --> CI{"⚙️ CI"}
    CI --> L["🔍 Lint"]
    CI --> T["🧪 Testes"]
    CI --> B["🏗️ Build /<br/>type-check"]
    L & T & B --> R{"tudo verde?"}
    R -->|sim| OK["✅ pode dar merge"]
    R -->|não| NO["❌ bloqueado"]

    style OK fill:#16a34a,color:#fff
    style NO fill:#dc2626,color:#fff
```

> 🔗 **A regra de ouro:** `ci.yml` e a skill `/verify-gates` devem andar em lockstep — o comando local
> que você roda é exatamente o que o CI aplica, então "verde localmente" significa "verde no CI."

---

## 🚢 6. Releases e Changelog

Os releases são automatizados por [`.github/workflows/release.yml`](.github/workflows/release.yml) e
seguem [Versionamento Semântico](https://semver.org/) + [Keep a Changelog](https://keepachangelog.com/).

```mermaid
flowchart LR
    CL["📝 Mover [Unreleased]<br/>→ vX.Y.Z no<br/>CHANGELOG.md"] --> TAG["🏷️ git tag vX.Y.Z<br/>git push origin vX.Y.Z"]
    TAG --> WF["⚙️ release.yml<br/>dispara"]
    WF --> REL["🎉 GitHub Release<br/>+ notas automáticas"]

    style REL fill:#d97706,color:#fff
```

Para publicar um release:

```bash
# 1. Mova as entradas de [Unreleased] no CHANGELOG.md sob um novo cabeçalho vX.Y.Z
# 2. Crie a tag e dê push — o workflow faz o resto
git tag v1.0.0
git push origin v1.0.0
```

> Tags de pré-release (`v1.0.0-rc.1`, `-beta.2`) são marcadas automaticamente como pré-release. As
> notas são geradas a partir dos commits/PRs desde a tag anterior.

---

## 🟢 7. Usando em um Projeto Real da Dataside

### 🆕 Começando um projeto novo a partir deste template

**Passo 1 — Crie o repositório.** Em `Dataside-Oficial`, clique em *"Use this template"* (mantenha
`main` como branch padrão), ou faça o scaffold localmente:

```bash
npx degit Dataside-Oficial/Dataside-DAIS-Template meu-app
cd meu-app
```

**Passo 2 — Preencha os placeholders.** Encontre cada token `{{...}}` e substitua:

```bash
grep -rl '{{' . --exclude-dir=.git
```

| Placeholder | Substitua por |
|-------------|---------------|
| `{{PROJECT_NAME}}` / `{{PROJECT_DESCRIPTION}}` | Nome e descrição em uma linha do projeto |
| `{{MAINTAINER}}` / `{{DATE}}` | Responsável e data de ratificação da Constituição |
| `{{PYTHON_DIR}}` / `{{NODE_DIR}}` / `{{FRONTEND_DIR}}` | Diretórios de trabalho por stack |
| `{{LINT_CMD}}` / `{{FORMAT_CMD}}` / `{{TEST_CMD}}` / `{{BUILD_CMD}}` | Seus comandos reais |

> #### ⚠️ Como preencher as variáveis — elas **não** são variáveis de ambiente
>
> Os tokens `{{...}}` são **placeholders literais embutidos nos arquivos do template** (`CLAUDE.md`,
> `.specify/constitution.md`, `.github/workflows/ci.yml`, `docs/…`). Você os substitui **uma vez, na
> mão**, ao iniciar o projeto — eles viram texto permanente no seu repositório, não configuração de
> runtime. Eles **nunca** vão em um `.env`.
>
> **Não confunda com secrets de runtime.** Chaves de API, URLs de banco e tokens *esses sim* vivem em
> um `.env` local e em **secrets do GitHub Actions** — nunca commitados (constituição §5). Resumindo:
>
> | Tipo | Exemplo | Vive em | Commitado? |
> |------|---------|---------|------------|
> | Placeholder do template | `{{PROJECT_NAME}}`, `{{TEST_CMD}}` | nos arquivos do repo (substituído 1×) | ✅ sim, como texto |
> | Secret de runtime | `OPENAI_API_KEY`, `DATABASE_URL` | `.env` local · secrets do CI no GitHub | ❌ nunca |

**Passo 3 — Escolha sua stack.** Em [`.github/workflows/ci.yml`](.github/workflows/ci.yml) e na skill
`/verify-gates`, **mantenha** os jobs de Python / Node / frontend que você usa, **apague** o resto e
preencha os comandos reais. (Um serviço FastAPI/LangGraph mantém o job de Python; um app React mantém
o de frontend; um app full-stack mantém os dois.)

**Passo 4 — Adapte a Constituição.** Mantenha §1–§6 (genéricos), mantenha ou apague §7 (bilíngue) e
**adicione** qualquer princípio específico do projeto — um contrato de protocolo de eventos, uma fonte
única da verdade de um modelo de dados, regras de provider.

**Passo 5 — Substitua os docs de placeholder.** `docs/architecture.md` descreve o *seu* sistema;
`CLAUDE.md` recebe os comandos e notas de arquitetura reais do projeto.

**Passo 6 — Escreva a spec `000`** e construa a primeira feature do jeito SDD + TDD. **Nunca pule
direto para o código.**

### ♻️ Adotando em um projeto já existente

Você não precisa recomeçar — traga a disciplina para dentro:

| Traga | Do template | Depois |
|-------|-------------|--------|
| Governança | `.specify/`, `specs/`, `docs/development-workflow.md` | Adapte a Constituição ao que o projeto já faz. |
| Porta do Claude Code | `.claude/`, `CLAUDE.md` | Una com qualquer `CLAUDE.md` existente; preencha os comandos reais. |
| Aplicação | `.github/workflows/` | Reconcilie com o CI existente; aponte os gates para os comandos reais de teste/lint. |

> ✅ **Regra de bolso para adoção:** o trabalho novo segue SDD + TDD desde o primeiro dia. Você não
> escreve specs retroativas para o código inteiro — você passa a escrever specs e testes para tudo
> que tocar daqui em diante.

---

## 🎨 8. Roadmap — Padrões Dataside

> 🚧 **Em breve.** Esta seção vai crescer até ser a casa dos **padrões de toda a Dataside**, para que
> projetos internos e de clientes saiam com a mesma identidade de engenharia. A intenção é que qualquer
> projeto criado a partir deste template seja *reconhecidamente Dataside* sem ninguém reinventar o
> básico:

- 🎨 **Identidade visual & frontend** — cores da Dataside, design tokens e padrões de componentes,
  para nenhuma UI sair fora da marca.
- 🔒 **Boas práticas de segurança** — tratamento de secrets, política de dependências, baseline de
  autenticação, LGPD/tratamento de dados em projetos de cliente.
- 📐 **Diretrizes de engenharia** — nomenclatura, estrutura de repositório, branching e convenções de
  revisão.
- ☁️ **Padrões de dados & cloud** — o jeito abençoado pela Dataside de entregar em Azure / Databricks /
  Snowflake / AWS (as plataformas em que a Dataside atua).
- 🤝 **Presets interno vs. cliente** — um interruptor leve para que uma entrega de cliente carregue o
  licenciamento, a titularidade e os docs de handover corretos, enquanto um projeto interno fica enxuto.

*Tem um padrão que toda a organização deveria compartilhar? Proponha aqui via PR — ele passa a fazer
parte do template que todo projeto novo herda.*

---

<div align="center">

**📜 A lei:** [`.specify/constitution.md`](.specify/constitution.md) ·
**🔄 O fluxo:** [`specs/README.md`](specs/README.md) ·
**📖 O guia:** [`docs/development-workflow.md`](docs/development-workflow.md)

<br />

Feito com disciplina na **Dataside** · [English](./README.md)

</div>
