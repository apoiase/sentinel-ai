# PRC — AWS / Runtime

Status em 2026-07-14. Validador: SRE. Base factual: ausência de `serverless.yml`/IaC no repo +
cruzamento do nome `sentinel` contra snapshot Steampipe pré-extraído desta task (Lambda, CFN,
S3, SQS, API Gateway REST, EventBridge, SNS — ver `docs/prc/scan/2026-07-14-scan.md`).

## Checklist

- [x] **AWS1** — Confirmado: **sem deploy AWS**. Repo é biblioteca (Python/PyPI + TS/npm), não
  serviço operado pela Apoia.se
- [x] **AWS2** — Zero recursos AWS com nome `sentinel` em Lambda/CFN/S3/SQS/APIGW/
  EventBridge/SNS (0 de 670 rows totais nesses domínios)
- [x] **AWS3** — Sem `serverless.yml`, `*.tf`, `template.y*ml` (CFN/SAM) ou `docker-compose*` no
  repo
- [x] **AWS4** — `Dockerfile` existe mas é self-host opcional do consumidor da lib (não há
  orquestração/manifest de deploy no repo) — não confirma nem contradiz uso interno da Apoia.se
  via container, apenas não há evidência de deploy operado por nós

## Exceções aceitas

| Item | Justificativa | Data |
|---|---|---|
| AWS1–4 | Repo é ferramenta de distribuição de pacote, não workload — checklist AWS não se aplica além da confirmação de ausência | 2026-07-14 |
