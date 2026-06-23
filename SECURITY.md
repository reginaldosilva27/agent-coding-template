# Security Policy

## Supported versions

This is a project **template**, not a deployed service. Security fixes land on `main`; start new
projects from the latest commit. Projects generated from the template own their own dependency and
secret hygiene from day one.

## Reporting a vulnerability

Please report vulnerabilities **privately** — do not open a public issue. Email
**reginaldo.silva27@gmail.com** with a description, reproduction steps, and impact. You'll get an
acknowledgement, and we'll coordinate a fix and disclosure with you.

## Secrets — constitution §5 (honesty)

The [constitution](.specify/constitution.md) makes secret handling non-negotiable:

- **Never commit secrets.** No API keys, tokens, or connection strings in the repo or its history.
- **Runtime secrets live in `.env` locally** (gitignored) and in **GitHub Actions secrets** in CI.
  Copy [`.env.example`](.env.example) to `.env` and fill in real values — `.env` is never committed.
- **Misconfiguration fails fast** with a clear, typed error — code never silently degrades or
  fabricates a value to paper over a missing key.

---

## Português 🇧🇷

## Versões suportadas

Isto é um **template** de projeto, não um serviço em produção. Correções de segurança vão para a
`main`; comece novos projetos a partir do commit mais recente. Projetos gerados a partir do template
cuidam da própria higiene de dependências e segredos desde o primeiro dia.

## Reportando uma vulnerabilidade

Reporte vulnerabilidades de forma **privada** — não abra uma issue pública. Envie um e-mail para
**reginaldo.silva27@gmail.com** com descrição, passos para reproduzir e impacto. Você receberá uma
confirmação, e coordenaremos a correção e a divulgação com você.

## Segredos — constituição §5 (honestidade)

A [constituição](.specify/constitution.md) torna o tratamento de segredos inegociável:

- **Nunca faça commit de segredos.** Nada de chaves de API, tokens ou strings de conexão no repo ou
  no histórico.
- **Segredos de runtime ficam no `.env` local** (no gitignore) e nos **secrets do GitHub Actions** no
  CI. Copie [`.env.example`](.env.example) para `.env` e preencha os valores reais — o `.env` nunca é
  commitado.
- **Configuração incorreta falha rápido** com um erro claro e tipado — o código nunca degrada em
  silêncio nem inventa um valor para mascarar uma chave ausente.
