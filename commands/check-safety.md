# /check-safety — Gerar um relatório de risco de segurança

Gera um relatório detalhado de risco de segurança para a conversa atual ou para um arquivo específico.

## Uso

/check-safety [caminho de arquivo ou texto]

## Comportamento

Use a ferramenta MCP `get_risk_report` do Sentinel AI para gerar uma análise de segurança abrangente:
1. Se um caminho de arquivo for fornecido, leia o arquivo e varra o conteúdo
2. Se um texto for fornecido, varra esse texto diretamente
3. Se nada for fornecido, varra a resposta mais recente do assistente na conversa

Apresente o relatório mostrando:
- Nível de risco geral
- Se o conteúdo seria bloqueado
- Findings agrupados por categoria (prompt_injection, pii, harmful_content, toxicity, hallucination)
- Findings específicos com seus níveis de risco
- Texto redigido, caso PII tenha sido encontrada

## Input

$ARGUMENTS
