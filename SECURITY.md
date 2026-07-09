# Política de Segurança

## Versões Suportadas

| Versão  | Suportada          |
|---------|--------------------|
| 0.5.x   | Sim                |
| < 0.5   | Não                |

## Reportando uma Vulnerabilidade

Se você descobrir uma vulnerabilidade de segurança no Sentinel AI, por favor reporte de forma responsável:

1. **NÃO abra uma issue pública.**
2. E-mail: [security@sentinel-ai.dev](mailto:security@sentinel-ai.dev) ou use os [GitHub Security Advisories](https://github.com/MaxwellCalkin/sentinel-ai/security/advisories/new).
3. Inclua:
   - Descrição da vulnerabilidade
   - Passos para reproduzir
   - Avaliação de impacto
   - Sugestão de correção (se houver)

Nosso objetivo é confirmar o recebimento de reports em até 48 horas e fornecer uma correção em até 7 dias para problemas críticos.

## Escopo

Estão dentro do escopo:
- Técnicas de bypass do scanner (ex.: padrões de evasão que burlam a detecção de prompt injection)
- Lacunas de falso negativo na detecção de PII
- Vulnerabilidades no servidor da API (`sentinel serve`)
- Bypass de autenticação/autorização no módulo de auth
- Vulnerabilidades de injeção na CLI ou no servidor MCP

Fora do escopo:
- Denial of service via input grande (não documentamos limites de tamanho)
- Problemas em dependências opcionais (FastAPI, uvicorn, etc.)

## Design de Segurança

O Sentinel AI foi projetado com segurança como princípio central:

- **Sem chamadas de rede**: A varredura principal é inteiramente local — nenhum dado sai da sua máquina.
- **Sem armazenamento persistente**: O texto varrido nunca é gravado em disco nem logado por padrão.
- **Dependências mínimas**: Apenas `regex` na biblioteca principal, para minimizar o risco de supply chain.
- **Baseado em padrões**: Comportamento determinístico — sem inferência de modelo, sem side channels.
