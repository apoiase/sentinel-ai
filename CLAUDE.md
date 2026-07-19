# sentinel-ai

Biblioteca Python (`sentinel-guardrails` no PyPI) + SDK TypeScript irmão de **guardrails de
segurança em tempo real para aplicações LLM** — scanners determinísticos (regex, sem GPU/API
externa) de prompt injection, PII, conteúdo nocivo, alucinação, toxicidade, código vulnerável e
segredos, com modos de deploy como proxy de API, proxy MCP ou hook do Claude Code.

> **Origem**: fork de [`MaxwellCalkin/sentinel-ai`](https://github.com/MaxwellCalkin/sentinel-ai)
> (autor terceiro, licença Apache 2.0), traduzido para pt-BR pela apoiase (PR #1 já mesclado).
> `ACQUISITION_PITCH.md`/`FOLLOWUP_EMAIL_DRAFT.md` (do autor upstream) indicam avaliação de
> aquisição/parceria com a Anthropic em andamento — fora do escopo deste rollout NA-598. Repo
> **PUBLIC**: nenhum recurso interno da Apoia.se (ARN, endpoint, secret) deve ser adicionado aqui.

## Comandos

```bash
pip install -e ".[dev,api]"      # dev: pytest + fastapi/uvicorn (extra "api")
python -m pytest tests/ -v       # suíte de testes (README cita 1641 testes)
sentinel scan "texto"            # CLI: scan direto
sentinel serve --port 8329       # sobe a API REST (extra "api")

cd sdk-js && npm install
npx tsx --test src/index.test.ts # testes do SDK TypeScript
npm run build
```

Sem deploy AWS — distribuição é o produto: PyPI (`publish.yml`, trusted publishing OIDC) e npm
(`publish-npm.yml`, token `NPM_TOKEN`), ambos disparados em `release`. Ver
`docs/prc/scan/2026-07-14-scan.md`.

## Arquitetura

`sentinel/` é um pacote **plano** com 188 módulos (só duas subpastas: `scanners/` e
`middleware/`) — camadas do knowledge graph (`.understand-anything/knowledge-graph.json`):

- **Core Engine** — `core.py` (`SentinelGuard.default().scan()`, ponto de entrada único),
  `policy.py`/`guard_policy.py` (configuração declarativa de quais scanners rodam).
- **Scanners nativos** (`sentinel/scanners/`) — 11 scanners: prompt injection, PII, harmful
  content, hallucination, toxicity, blocked terms, tool use, structured output, code, obfuscation,
  secrets. `secrets_scanner.py` usa a mesma classe de regra que os gates deste rollout
  (gitleaks/Trivy), reimplementada em Python.
- **Modos de deployment** — `proxy.py` (firewall reverso na frente de qualquer API LLM),
  `mcp_proxy.py` (proxy de segurança pra qualquer servidor MCP), `hooks.py` +
  `hooks/pre-tool-scan.sh`/`post-tool-scan.sh` (hook do Claude Code, registrado via
  `.claude-plugin/plugin.json`), `mcp_server.py` (expõe o próprio Sentinel como servidor MCP,
  ver `.mcp.json`), `api.py` (REST).
- **Integrações** — wrappers "drop-in" (`anthropic_wrapper.py`, `openai_wrapper.py`) e callbacks
  (`middleware/langchain_callback.py`, `middleware/llamaindex_callback.py`,
  `middleware/fastapi_middleware.py`, `middleware/agent_sdk.py`); `sdk-js/` espelha a mesma API
  em TypeScript.
- **Governança/auditoria** — `compliance.py`, `audit_log.py`, `session_guard.py`/
  `session_replay.py`, `cost_tracker.py`, `attack_chain.py`, `threat_intel.py`.
- **CLI e distribuição** — `cli.py` (entry point `sentinel`), `.claude-plugin/` + `commands/` +
  `hooks/` (plugin Claude Code publicado no marketplace), `Dockerfile` (self-host opcional, sem
  orquestração no repo).

**Observação de arquitetura**: `sentinel/__init__.py` reexporta ~60 símbolos e há sobreposição de
nome entre módulos (5 de "compliance": `compliance.py`/`compliance_gate.py`/
`compliance_checker.py`/`compliance_report.py`/`compliance_audit.py`; 2 de "threat_intel(ligence)";
dezenas de `prompt_*.py`/`output_*.py` com responsabilidades próximas) — sinal de sprawl de API
surface para uma lib que se posiciona como "leve, sem dependências pesadas". Não investiguei
duplicação funcional a fundo (fora do escopo do mapeamento big-picture).

## Contexto de domínio

Não é domínio de negócio da Apoia.se (cobrança/apoio/repasse) — é uma ferramenta de
AI-safety/tooling adotada pela organização. Sem referência ao `apoiase-context-layer`.

## Convenções

- Commits: português natural, com `NA-598` no texto durante o rollout. Branch:
  `big-picture/NA-598-*`.
- Repo GitHub (SoT, fork ativo). Merge **só pela UI do GitHub** com review humano (nunca API/
  `gh pr merge`).
- CI existente preservado **verbatim**: `.github/workflows/ci.yml` (testes),
  `publish.yml` (PyPI), `publish-npm.yml` (npm). Gate novo deste rollout:
  `.github/workflows/quality-gates.yml`, 100% **consultivo** (`continue-on-error: true` em todo
  step — nunca bloqueia PR nem publish).
- Repo **PUBLIC**: cuidado redobrado — nada de ARN de conta AWS, secret real ou endpoint interno
  da Apoia.se em qualquer arquivo novo.

## Knowledge graph

- `.understand-anything/knowledge-graph.json` — montado à mão (repo grande, sem pipeline de
  subagents por instrução do rollout GH): 50 nós, 49 edges, 7 layers, tour de 14 passos.
- `graphify-out/graph.json` — **não existe**, não gerado nesta sessão (ver scan).

## Production Readiness

Checklists por escopo em `docs/prc/`. Resumo do scan de 2026-07-14: sem `serverless.yml`/IaC,
zero recursos AWS com nome `sentinel` (Lambda/CFN/S3/SQS/APIGW/EventBridge/SNS — checado contra
snapshot Steampipe da task), deploy real = publicação de pacote (PyPI/npm) via CI. Achado de
segurança: 28 findings de `gitleaks` no HEAD, **todos fixtures de teste** do próprio
`secrets_scanner.py`/`benchmarks.py` (credenciais de exemplo, não reais) — mapeados em
`.gitleaksignore` da raiz da task, não rotacionados (nada real a rotacionar).
