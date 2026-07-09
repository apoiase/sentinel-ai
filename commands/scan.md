# /scan — Varrer texto em busca de problemas de segurança

Varre o texto fornecido em busca de problemas de segurança usando todos os scanners do Sentinel AI.

## Uso

/scan <texto a varrer>

## Comportamento

Execute os 7 scanners de segurança do Sentinel AI contra o texto fornecido:
- Detecção de prompt injection
- Detecção e redação de PII
- Filtragem de conteúdo prejudicial
- Detecção de sinais de alucinação
- Detecção de toxicidade
- Verificação de termos bloqueados
- Análise de segurança de tool-use

Reporte os findings com níveis de risco (NONE, LOW, MEDIUM, HIGH, CRITICAL) e se o conteúdo deveria ser bloqueado.

Se PII for detectada, mostre também a versão redigida do texto.

## Input

$ARGUMENTS
