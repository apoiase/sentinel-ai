# PRC — Observability (logs, erros, alertas, métricas)

Status em 2026-07-14. Validador: SRE.

## Checklist

- [x] **OBS1** — `sentinel/audit_log.py` (`AuditLog`/`AuditEvent`) — trilha de decisões do guard,
  parte da API pública da lib (não é observabilidade do próprio repo, é feature que a lib
  oferece ao consumidor)
- [x] **OBS2** — `sentinel/decision_logger.py`, `sentinel/interaction_logger.py`,
  `sentinel/session_audit.py`/`session_replay.py` — mesma categoria: logging/replay é
  funcionalidade do produto, não instrumentação operacional deste repo
- [ ] **OBS3** — Sem OpenTelemetry/coletor configurado no CI ou runtime do próprio repo (não se
  aplica — não há serviço rodando 24/7 sob operação da Apoia.se; ver PRC aws)
- [ ] **OBS4** — Sem alerta configurado para falha de publish (PyPI/npm) além do status do
  workflow no GitHub Actions
- [ ] **OBS5** — `sentinel/telemetry.py` existe como módulo da lib (feature exposta ao
  consumidor via `examples/observability.py`) — não confundir com telemetria própria deste repo

## Exceções aceitas

| Item | Justificativa | Data |
|---|---|---|
| OBS3–5 | Repo distribui pacote, não opera serviço; observabilidade é feature do produto para o consumidor, não requisito operacional deste repo | 2026-07-14 |
