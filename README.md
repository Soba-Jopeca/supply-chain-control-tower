# Supply Chain Returnables Control Tower

Plataforma de dados (Data Product) para antecipar risco operacional em transferências de materiais e ativos retornáveis, integrando ERP, TMS, WMS, GPS e Assets — construída como projeto de portfólio de Data Engineering, simulando um cenário corporativo real (Azure + Databricks).

> Dados → Informação → Risco → Decisão → Ação → Resultado → Valor

## Por que este projeto existe

Ver o problema de negócio completo em [`docs/business/problem_statement.md`](docs/business/problem_statement.md). Resumo: hoje a detecção de atrasos e rupturas de ativos retornáveis é reativa porque os dados estão fragmentados entre sistemas que não conversam. O objetivo é transformar isso em um processo preventivo com pelo menos 6h de antecedência (premissa a validar).

## Ambiente

| | Arquitetura alvo (corporativa) | Implementação de laboratório (este repo) |
|---|---|---|
| Compute | Azure Databricks | Databricks Free Edition |
| Storage | ADLS Gen2 | Unity Catalog Volumes / storage padrão |
| Orquestração externa | Azure Data Factory | Databricks Workflows |
| Secrets | Azure Key Vault | Databricks secret scopes |
| BI | Power BI | Power BI (via Databricks SQL) |

Detalhes completos e justificativa: [`docs/adr/ADR-0002-lab-vs-target-environment.md`](docs/adr/ADR-0002-lab-vs-target-environment.md).

## Estrutura do projeto

```text
config/            # configuração por ambiente (dev/test/prod)
src/               # código Python reutilizável (ingestion, transformations, quality, monitoring, risk, utils)
pipelines/         # notebooks/jobs finos por camada medallion (bronze/silver/gold)
data_generator/    # geradores de dados sintéticos por sistema fonte
tests/             # unit, integration, data_quality
resources/         # definição de jobs/pipelines para Databricks Asset Bundles
infrastructure/    # Terraform (IaC)
docs/              # arquitetura, negócio, modelo de dados, ADRs, runbooks, performance
.github/workflows/ # CI/CD
```

Estrutura completa e objetivos técnicos detalhados: [`docs/planning/00-master-prompt-and-description.md`](docs/planning/00-master-prompt-and-description.md).

## Roadmap

O projeto é executado em fases (0 a 25), documentadas em [`docs/planning/01-roadmap-fases.md`](docs/planning/01-roadmap-fases.md). Progresso e status ficam registrados em `CHANGELOG.md` e nos ADRs.

## Como rodar (lab)

1. Criar workspace no [Databricks Free Edition](https://www.databricks.com/learn/free-edition).
2. Clonar este repositório via Git folder do Databricks ou localmente + Databricks CLI.
3. Instalar dependências: `pip install -e ".[dev]"` (ver `pyproject.toml`).
4. Rodar os geradores de dados sintéticos (`data_generator/`) para popular a camada Landing.
5. Deploy do Asset Bundle: `databricks bundle deploy -t dev` (ver `databricks.yml`).

## Definition of Done (por funcionalidade)

Ver checklist completo em [`docs/planning/00-master-prompt-and-description.md`](docs/planning/00-master-prompt-and-description.md#20-definition-of-done). Resumo: código + testes + qualidade validada + documentação + observabilidade + idempotência + tratamento de erro + config externalizada + PR-ready.

## Licença

MIT — ver [`LICENSE`](LICENSE).
