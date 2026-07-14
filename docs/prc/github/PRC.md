# PRC — GitHub (repo, Actions, fluxo de PR)

Status em 2026-07-14. Validador: SRE.

## Checklist

- [x] **GH1** — CI existente (`ci.yml`) roda testes em push/PR — preservado verbatim
- [x] **GH2** — Publish automatizado via `release` (PyPI `publish.yml` com trusted publishing
  OIDC, npm `publish-npm.yml` com `NPM_TOKEN`) — preservado verbatim
- [x] **GH3** — Quality gates de PR novo (`.github/workflows/quality-gates.yml`, criado neste
  rollout, gitleaks + Semgrep + Trivy): **todos consultivos** (`continue-on-error: true` — nunca
  bloqueia)
- [ ] **GH4** — Repo é **fork** ativo de `MaxwellCalkin/sentinel-ai` (upstream terceiro) — sem
  processo definido de rebase/sync periódico com upstream; risco de divergir e perder correções
  de segurança do scanner original
- [ ] **GH5** — Branch protection na `main`: não verificável só pelo código — checar via API/UI
- [ ] **GH6** — Sem template de PR (`.github/pull_request_template.md`) — recomendação aberta
- [ ] **GH7** — Renovate/Dependabot: não configurado; GitHub suporta nativamente (diferente do
  Bitbucket, onde é bloqueado por auth — não é o caso aqui), recomendação aberta
- [ ] **GH8** — Repo **PUBLIC**: `ACQUISITION_PITCH.md`/`FOLLOWUP_EMAIL_DRAFT.md` (herdados do
  upstream) expõem contato/negociação pessoal do autor original com a Anthropic — não é secret
  técnico, mas fora do escopo deste rollout tratar (conteúdo do fork upstream, não introduzido
  pela Apoia.se)

## Exceções aceitas

| Item | Justificativa | Data |
|---|---|---|
| — | — | — |
