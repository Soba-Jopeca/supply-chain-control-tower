# ADR-0001: Arquitetura Lakehouse com padrão Medallion (Bronze/Silver/Gold)

**Status:** Accepted
**Data:** Sprint 0
**Fase relacionada:** Fase 1 - Solution Architecture

## Context

O discovery (Sprint 0) identificou que os dados de ERP, TMS, WMS, GPS e Assets são fragmentados, chegam com qualidade e frequência diferentes, e precisam ser reconciliados sem perder a origem para fins de auditoria. Precisamos de um padrão de organização de dados que suporte evolução incremental de confiabilidade sem reprocessar tudo do zero a cada mudança de regra de negócio.

## Decision

Adotar a arquitetura Lakehouse com o padrão Medallion:

- **Bronze**: dado bruto, o mais próximo possível da origem, sem correção de semântica de negócio.
- **Silver**: dado tratado, deduplicado, padronizado, com integridade referencial e regras de negócio aplicadas — Trusted Data Layer.
- **Gold**: Data Products orientados a negócio (Shipment Control Tower, Inventory in Transit, Route Performance, Carrier Performance, Returnables Balance, Operational Risk).

Formato de armazenamento: Delta Lake em todas as camadas (ACID, schema enforcement/evolution, MERGE, time travel).

## Alternatives

- **Data Warehouse tradicional (schema-on-write único)**: descartado porque não suporta bem a ingestão de dados semiestruturados/inconsistentes (GPS, planilhas) sem uma camada bruta intermediária.
- **Data Lake sem camadas (data swamp)**: descartado — sem separação Bronze/Silver/Gold não há como garantir confiabilidade progressiva nem auditar transformações.
- **ELT direto para Gold**: descartado — perderíamos a capacidade de replay/backfill a partir do dado original quando uma regra de negócio mudar.

## Consequences

- Cada camada tem responsabilidade única e testável isoladamente.
- Custo de armazenamento maior (dado duplicado entre camadas) — aceitável no contexto do projeto.
- Exige disciplina de não "pular camada" (ex: Gold não pode ler direto de Bronze).
- Serve de base direta para a Fase 4 (Bronze), Fase 5 (Silver) e Fase 8 (Gold) do roadmap.

## Status no ambiente de laboratório

`Implemented in lab` — usando Databricks Free Edition + Delta Lake + Unity Catalog (schemas `bronze`, `silver`, `gold` dentro do catalog do projeto).
