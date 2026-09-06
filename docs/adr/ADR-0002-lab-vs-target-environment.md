# ADR-0002: Separação explícita entre Arquitetura Alvo (corporativa) e Implementação de Laboratório

**Status:** Accepted
**Data:** Sprint 0 / Fase 1
**Fase relacionada:** Fase 1 - Solution Architecture

## Context

A arquitetura alvo do projeto assume um ambiente corporativo Azure completo (ADF, ADLS Gen2, Azure Databricks, Key Vault, Event Hubs, Azure Monitor). Na prática, o ambiente disponível hoje é o **Databricks Free Edition** (serverless-only, quota-limitado, sem custom compute, outbound restrito a domínios confiáveis, 1 SQL warehouse 2X-Small, máx. 5 job tasks concorrentes, 1 pipeline ativo por tipo). Não é honesto nem produtivo fingir capacidades que o laboratório não tem.

## Decision

Cada componente da arquitetura será documentado com uma de três tags, sempre visível no ADR ou no README do módulo correspondente:

- `Implemented in lab` — funciona de fato no Databricks Free Edition hoje.
- `Target production implementation` — existe na arquitetura alvo (ex: ADF para orquestração de ingestão externa, Event Hubs para streaming), mas não é implementado no lab; documentado conceitualmente com diagrama e justificativa técnica.
- `Demonstrated conceptually` — simulado de forma simplificada no lab só para provar o conceito (ex: Key Vault → variável de ambiente/secret scope do Databricks Free Edition).

Mapeamento inicial:

| Componente arquitetura alvo | Implementação no lab |
|---|---|
| ADLS Gen2 | `Demonstrated conceptually` — Unity Catalog Volumes / storage padrão do Free Edition |
| Azure Data Factory (orquestração de ingestão externa) | `Target production implementation` — no lab, orquestração via Databricks Workflows/Jobs |
| Azure Databricks | `Implemented in lab` — via Databricks Free Edition (mesma UI/API) |
| Azure Key Vault | `Demonstrated conceptually` — Databricks secret scopes |
| Azure Event Hubs (streaming) | `Target production implementation` — não implementado; fora do escopo (SLA diário validado em discovery) |
| Azure Monitor | `Demonstrated conceptually` — logs estruturados + tabelas de observabilidade próprias |
| Power BI | `Implemented in lab` — conecta via Databricks SQL, mesmo endpoint usado em produção |
| Terraform | `Demonstrated conceptually` — módulos escritos e documentados; execução real depende de credenciais de conta que o Free Edition não expõe (sem account console/API) |

## Alternatives

- Ignorar a limitação e "fingir" Azure completo: descartado — gera portfólio pouco confiável em entrevista técnica (perguntas de profundidade expõem a lacuna).
- Esperar renovar o Azure free trial antes de começar: descartado — bloqueia o aprendizado prático agora; o Databricks Free Edition já cobre ~80% do valor técnico do projeto.

## Consequences

- Todo componente do projeto tem status de maturidade explícito e defensável em entrevista.
- Quando o Azure trial for renovado (ou usado o ambiente da empresa), plugamos ADLS Gen2/ADF por cima sem redesenhar a lógica de Bronze/Silver/Gold.
- Exige disciplina de sempre atualizar a tag quando um componente evoluir de "conceitual" para "implementado".

## Status no ambiente de laboratório

`Implemented in lab` (esta é a própria decisão sobre como tratar o lab).
