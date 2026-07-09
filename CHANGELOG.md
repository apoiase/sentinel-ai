# Changelog

Todas as mudanças notáveis deste projeto serão documentadas neste arquivo.

## [0.8.4] - 2026-03-08

### Adicionado
- **Secrets Scanner** — `sentinel secrets-scan` detecta credenciais hardcoded no código-fonte
  - Mais de 40 padrões: AWS, GitHub, Google, Stripe, Slack, OpenAI, Anthropic, Twilio, SendGrid, npm, PyPI, Azure, Firebase, Supabase, Mailgun
  - Detecção de chaves privadas (RSA, EC, PGP, OpenSSH)
  - Padrões genéricos de segredos (senhas, API keys, tokens, bearer tokens)
  - Strings de conexão de banco de dados com credenciais embutidas
  - Análise de entropia de Shannon para reduzir falsos positivos
  - Filtragem de falsos positivos (placeholders, variáveis de ambiente, templates)
  - Varredura com reconhecimento de comentários
  - Varredura de diretórios com filtragem por tipo de arquivo
  - Integrado ao `sentinel project-scan` e à GitHub Action
  - 66 novos testes
- GitHub Action: novo input `secrets-scan` para detecção de segredos em CI/CD
- Contagem de testes: 743 → 809

## [0.8.3] - 2026-03-08

### Adicionado
- **MCP Tool Schema Validator** — `sentinel mcp-validate` detecta vetores de injeção em definições de ferramentas MCP
  - Prompt injection, personificação de autoridade, exfiltração de dados em descrições de ferramentas
  - Valores padrão suspeitos de parâmetros (URLs, comandos shell)
  - Conteúdo oculto (comentários HTML, caracteres de largura zero)
  - Validação de schema aninhado (properties, anyOf, items)
  - 40 novos testes
- Contagem de testes: 687 → 727

## [0.8.2] - 2026-03-08

### Adicionado
- **Dependency Scanner** — `sentinel dep-scan` detecta ataques de supply chain em arquivos de dependências
  - Detecção de typosquatting para mais de 20 pacotes populares de Python/JS
  - Base de dados de pacotes maliciosos conhecidos (mais de 50 pacotes removidos dos registries)
  - Detecção de URLs suspeitas (paste sites, IPs crus, TLDs não confiáveis)
  - Scripts de instalação perigosos no package.json (curl|bash, wget, etc.)
  - Avisos de version pinning (não fixado, wildcard, limites frouxos)
  - Suporta: requirements.txt, package.json, pyproject.toml, Pipfile
  - Auto-detecção de arquivos de dependências no diretório do projeto
  - Saída em JSON para integração com CI/CD
- 36 novos testes do dependency scanner

### Alterado
- Contagem de testes: 651 → 687

## [0.8.1] - 2026-03-08

### Adicionado
- **Security Audit** — `sentinel audit` pontua a configuração de segurança do projeto (0-100) em 6 verificações: hooks do Claude Code, allowlist de permissões, política de segurança, arquivos .env, hooks de pre-commit do git, configuração do MCP
  - Saída em JSON para integração com CI/CD (`--format json`)
  - Retorna código de saída 1 se problemas críticos forem encontrados
- **CLAUDE.md Scanner** — `sentinel claudemd-scan` detecta 11 categorias de vetores de injeção em arquivos de instrução do projeto
  - Varre CLAUDE.md, .cursorrules, .github/copilot-instructions.md
  - Detecta: injeção via comentário HTML, personificação de autoridade, override de base URL, permissões perigosas, desativação de segurança, comandos de exfiltração, comandos destrutivos, caracteres de largura zero, payloads em base64, homoglifos, execução arbitrária de código
  - 36 novos testes do scanner de CLAUDE.md
- **FastAPI Middleware** — `create_sentinel_middleware()` para apps FastAPI/Starlette
  - 14 novos testes de middleware
- 31 novos testes de auditoria

### Alterado
- Contagem de testes: 570 → 651

## [0.8.0] - 2026-03-08

### Adicionado
- **Saída SARIF v2.1.0** — Gera formato SARIF para GitHub Code Scanning, Azure DevOps e outras ferramentas de análise estática
  - Suporte via CLI a `sentinel scan --format sarif` e `sentinel code-scan --format sarif`
  - API Python `scan_result_to_sarif()` e `findings_to_sarif()`
  - Mapeia RiskLevel para níveis de severidade SARIF, spans de Finding para regiões SARIF
  - Inclui deduplicação de regras e propagação de metadados
- Opção `upload-sarif` da GitHub Action para integração automática com o GitHub Code Scanning
- 22 novos testes de SARIF

### Alterado
- Contagem de testes: 536 → 558

## [0.7.1] - 2026-03-08

### Adicionado
- **Streaming Safety Scanning** — `guarded_stream()` e `guarded_stream_async()` encapsulam a API de streaming da Anthropic com varredura de saída em tempo real via `StreamingGuard`. Bloqueia conteúdo perigoso no meio do stream.
- **Suíte de Testes do Anthropic SDK Wrapper** — 30 testes cobrindo `guarded_message`, `guarded_stream`, varredura de tool use, redação de PII e variantes assíncronas
- Helper compartilhado `_scan_inputs()` para reduzir duplicação de código no wrapper

### Alterado
- Contagem de testes: 481 → 511

## [0.7.0] - 2026-03-08

### Adicionado
- **Obfuscation Detection Scanner** — Detecta payloads de ataque codificados/ofuscados:
  - Conteúdo malicioso codificado em base64 (injeção de comando, injeção SQL, override de prompt)
  - Comandos codificados em hex (sequências de escape `\x`, formato `0x`)
  - Instruções perigosas codificadas em ROT13
  - Sequências de escape Unicode escondendo payloads de ataque
  - Variantes em leetspeak de termos perigosos (ex.: `1gn0r3 1n5truct10n5`)
- 24 novos testes para detecção de ofuscação

### Alterado
- Contagem de scanners: 9 → 10
- Contagem de testes: 457 → 481

## [0.6.0] - 2026-03-07

### Adicionado
- **Code Vulnerability Scanner** — Detecção do OWASP Top 10 em código gerado por LLM:
  - Injeção SQL (f-strings, concatenação de strings, `.format()`)
  - Injeção de comando (`shell=True`, `os.system`, `eval`)
  - XSS (`innerHTML`, `document.write`, Jinja2 inseguro)
  - Path traversal, desserialização insegura, segredos hardcoded
  - Criptografia insegura (MD5, SHA1, AES-ECB)
  - SSRF (URLs controladas pelo usuário em requests)
- **MCP Safety Proxy** — `sentinel mcp-proxy` encapsula qualquer servidor MCP com varredura de segurança
  - Varre todos os argumentos de ferramentas antes de encaminhar
  - Auto-redação de PII nas respostas de ferramentas
  - Bloqueia operações perigosas na camada de transporte
- Comandos CLI: `sentinel code-scan`, `sentinel mcp-proxy`
- Suporte da GitHub Action para varredura de código (input `code-scan`)
- Política de divulgação responsável em SECURITY.md
- 46 novos testes (33 do code scanner + 13 do MCP proxy)

## [0.5.2] - 2026-03-06

### Adicionado
- Detecção de vetores de ataque do Claude Code (CVE-2025-59536, CVE-2026-21852)
- Detecção multilíngue de prompt injection (12 idiomas)
- Site de demo ao vivo em maxwellcalkin.github.io/sentinel-ai

## [0.5.0] - 2026-03-05

### Adicionado
- Servidor MCP com 7 ferramentas de segurança
- Scanner de segurança para streaming (`StreamingGuard`)
- Guard em nível de conversa
- Framework de testes adversariais
- Gerador de relatório de risco RSP
- Motor de políticas com configuração YAML/JSON
- Workflow de CI/CD via GitHub Actions
- Suíte de benchmark com 530 casos

## [0.4.0] - 2026-03-04

### Adicionado
- Scanner de tool-use para segurança de MCP/function-calling
- Validador de saída estruturada
- Scanner de termos bloqueados
- Integração de hook do Claude Code (`sentinel hook`)

## [0.3.0] - 2026-03-03

### Adicionado
- Detector de alucinação (linguagem de hedging, pontuação de confiança)
- Scanner de toxicidade
- Scanner de conteúdo prejudicial (armas, drogas, autolesão, atividade ilegal)

## [0.2.0] - 2026-03-02

### Adicionado
- Scanner de PII com auto-redação (e-mails, SSNs, cartões de crédito, telefones, endereços IP)
- Servidor FastAPI para deploy via API REST
- Suporte a varredura assíncrona

## [0.1.0] - 2026-03-01

### Adicionado
- Lançamento inicial
- Scanner de detecção de prompt injection
- Motor de orquestração central (`SentinelGuard`)
- `ScanResult` com níveis de risco (none/low/medium/high/critical)
