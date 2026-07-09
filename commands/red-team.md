# /red-team — Testar a robustez da varredura de segurança

Faça red-team da sua varredura de segurança gerando variantes adversariais de uma tentativa de prompt injection.

## Uso

/red-team <texto da tentativa de injeção>

## Comportamento

Use a ferramenta MCP `test_robustness` do Sentinel AI para:
1. Pegar o texto fornecido (uma tentativa de injeção conhecida)
2. Gerar variantes adversariais usando mais de 10 técnicas de evasão (homoglifos, caracteres de largura zero, leetspeak, divisão de payload, substituição de sinônimos, etc.)
3. Testar se cada variante ainda é detectada pelo scanner de segurança
4. Reportar a taxa de detecção, o total de variantes testadas e quaisquer variantes que evadiram a detecção

Apresente os resultados mostrando:
- Taxa de detecção geral (ex.: 95%)
- Número de variantes testadas vs. detectadas
- Lista das técnicas de evasão usadas
- Quaisquer variantes que evadiram a detecção (com a técnica usada)

Isso ajuda a identificar fraquezas na varredura de segurança antes que possam ser exploradas.

## Input

$ARGUMENTS
