# PRC — Security (scanning, higiene de secrets, acesso)

Status em 2026-07-14. Validadores: SEC + SRE. Repo **PUBLIC** — atenção redobrada.

## Checklist

- [x] **SEC1** — Secret scanning no PR (gitleaks, `.github/workflows/quality-gates.yml`, criado
  neste rollout)
- [x] **SEC2** — SAST no PR (Semgrep, `p/security-audit` — ajustar config pra incluir
  `p/python` e `p/typescript` dado o stack misto)
- [x] **SEC3** — SCA no PR (Trivy fs)
- [x] **SEC4** — Higiene de secrets: `gitleaks detect` local no HEAD encontrou **28 findings**,
  **todos fixtures sintéticas de teste** do próprio scanner de segredos da lib, confirmadas
  manualmente (não são credenciais reais):
  - `sentinel/benchmarks.py` (17 findings) — 600 casos de benchmark do README, incluem exemplos
    de `sk-proj-...`, `ghp_...`, `AIzaSy...`, chave AWS `...EXAMPLEKEY01`, JWT de exemplo, RSA/
    OpenSSH private key de exemplo — literais necessários pra testar detecção
  - `sentinel/scanners/secrets_scanner.py` (2) — regex patterns de private key (não segredos)
  - `tests/test_secrets_scanner.py` (5) — fixtures de teste unitário (Mailgun/GCP/private-key/
    generic-api-key de exemplo)
  - `tests/test_config_validator.py`, `tests/test_prompt_firewall.py`,
    `tests/test_code_scanner.py` (1 cada) — mesma categoria
  - `sdk-js/src/index.test.ts` (2) — espelha os testes Python no SDK TS (Stripe/generic key de
    exemplo)
  - `docs/index.html`, `site/index.html` (1 cada) — demo estática, mesmo texto de exemplo
  - `examples/code_vulnerability_scan.py` (1) — exemplo de uso do CodeScanner
  - Mapeados no `.gitleaksignore` da **raiz da task** (fingerprint `path:rule:line`, path
    absoluto do clone) — **NÃO rotacionado** (não há credencial real a rotacionar)
- [ ] **SEC5** — gitleaks só roda no PR (novo gate), não como pre-commit hook local
- [ ] **SEC6** — Fork de terceiro (`MaxwellCalkin/sentinel-ai`) sem processo de sync/rebase
  periódico — correções de segurança do scanner original podem não chegar automaticamente
  (mesmo achado registrado em GH4 do PRC github)
- [ ] **SEC7** — Baseline de dependências (Trivy): não triado nesta rodada; `dependencies` do
  `pyproject.toml` é enxuto (só `regex`), risco concentrado nos extras opcionais
  (`torch`/`transformers` em `[ml]`, não instalados por padrão)
- [x] **SEC8** — Publish PyPI usa **Trusted Publishing (OIDC)**, sem token estático — boa prática
  já presente no repo, não introduzida por este rollout
- [ ] **SEC9** — Publish npm usa `NPM_TOKEN` estático (`secrets.NPM_TOKEN`) — considerar migrar
  pra npm Trusted Publishing (OIDC) quando disponível pro pacote
- [x] **SEC10** — Repo é **PUBLIC**: nada de ARN de conta AWS, secret real ou endpoint interno da
  Apoia.se foi introduzido pelos artefatos deste rollout (CLAUDE.md, PRCs, scan, knowledge graph
  — todos revisados antes do commit)
- [ ] **SEC11** — Renovate/Dependabot não configurado; GitHub suporta nativamente — recomendação
  aberta

## Exceções aceitas

| Item | Justificativa | Data |
|---|---|---|
| SEC4 (28 findings) | Fixtures sintéticas do próprio secrets_scanner/benchmarks — confirmadas manualmente como exemplos, não credenciais reais; necessárias para a lib testar sua própria detecção | 2026-07-14 |
