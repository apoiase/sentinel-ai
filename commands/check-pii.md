# /check-pii — Verificar texto em busca de PII e redigir

Verifica um texto ou arquivo em busca de informações pessoalmente identificáveis e mostra a versão redigida.

## Uso

/check-pii <texto ou caminho de arquivo>

## Comportamento

Use a ferramenta MCP `check_pii` do Sentinel AI para:
1. Varrer o texto fornecido em busca de PII (e-mails, SSNs, cartões de crédito, números de telefone, API keys, tokens)
2. Reportar quais tipos de PII foram encontrados
3. Mostrar a versão redigida com PII substituída por labels como [EMAIL], [SSN], [CREDIT_CARD]

Se um caminho de arquivo for fornecido no lugar de texto, leia o arquivo primeiro e depois varra o conteúdo.

## Input

$ARGUMENTS
