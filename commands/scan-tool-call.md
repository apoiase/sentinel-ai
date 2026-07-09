# /scan-tool-call — Verificar se uma chamada de ferramenta é segura

Varre uma chamada de ferramenta em busca de operações perigosas antes da execução.

## Uso

/scan-tool-call <tool_name> <argumentos em JSON>

## Comportamento

Use a ferramenta MCP `scan_tool_call` do Sentinel AI para:
1. Fazer parse do nome da ferramenta e dos argumentos em JSON
2. Verificar comandos shell perigosos (rm -rf, acesso a credenciais, etc.)
3. Verificar padrões de exfiltração de dados (curl para servidores externos, etc.)
4. Verificar tentativas de escalonamento de privilégio
5. Reportar o nível de risco e se a chamada deveria ser bloqueada

Apresente os findings com:
- Nível de risco (NONE, LOW, MEDIUM, HIGH, CRITICAL)
- Tipo de ameaça (dangerous_command, exfiltration, sensitive_file, privilege_escalation)
- Descrição da ameaça
- Se a chamada de ferramenta seria bloqueada

## Input

$ARGUMENTS
