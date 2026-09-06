# Roadmap do projeto

## Fase 0 — Discovery

### Objetivo

Entender o problema antes da solução técnica.

### Entregáveis

- Problem Statement;
- Stakeholder Map;
- Current State;
- Future State;
- Functional Requirements;
- Non-Functional Requirements;
- Business Rules;
- Source Inventory;
- Data Contracts;
- SLA;
- RACI;
- Open Questions;
- Initial Architecture;
- Initial Backlog.

### Status

Concluída parcialmente.

---

# Fase 1 — Solution Architecture

## Objetivo

Transformar o problema de negócio em arquitetura técnica.

### Atividades

Definir:

- arquitetura Lakehouse;
- ingestão;
- armazenamento;
- processamento;
- governança;
- serving;
- observabilidade;
- segurança;
- CI/CD.

### Entregáveis

```text
Architecture Diagram
Context Diagram
Data Flow
Deployment Architecture
Security Model
Data Lifecycle
ADR
```

### Pergunta que deve responder

> Como transformar as necessidades descobertas no Sprint 0 em uma plataforma técnica sustentável?

---

# Fase 2 — Synthetic Data Engineering

## Objetivo

Construir fontes de dados realistas.

### Sistemas simulados

```text
ERP
TMS
WMS
GPS
ASSETS
MASTER DATA
```

### Características

Os dados devem conter:

- duplicidade;
- atraso;
- inconsistência;
- schema drift;
- registros inválidos;
- eventos fora de ordem;
- missing data;
- correções;
- cancelamentos.

### Entregáveis

- generators;
- schemas;
- data contracts;
- documentation;
- reproducibility;
- seeds;
- volume profiles.

---

# Fase 3 — Ingestion / Landing

## Objetivo

Construir a primeira camada de ingestão.

### Conceitos

- batch;
- incremental;
- metadata;
- ingestion timestamp;
- source file;
- batch ID;
- record hash;
- idempotency.

### Metadados esperados

```text
_ingestion_timestamp
_source_system
_source_file
_batch_id
_record_hash
_ingestion_date
```

### Entregável

Dados disponíveis na camada Bronze.

---

# Fase 4 — Bronze Lakehouse

## Objetivo

Preservar os dados recebidos da origem.

### Princípio

Bronze deve ser próxima do dado original.

Não corrigir semântica de negócio nessa camada.

### Demonstrar

- Delta;
- schema enforcement;
- schema evolution;
- metadata;
- replay;
- idempotency.

---

# Fase 5 — Silver / Trusted Data

## Objetivo

Transformar dados brutos em dados confiáveis.

### Operações

- casting;
- normalization;
- deduplication;
- standardization;
- validation;
- referential integrity;
- business rules.

### Principais entidades

```text
shipments
shipment_items
transport_events
inventory_movements
gps_events
plants
materials
routes
carriers
vehicles
assets
```

### Resultado

Trusted Data Layer.

---

# Fase 6 — Data Quality

## Objetivo

Transformar qualidade de dados em parte explícita da engenharia.

### Criar

- expectations;
- quality rules;
- quarantine;
- quality score;
- quality report.

### Métricas

```text
Completeness
Validity
Uniqueness
Consistency
Freshness
```

### Resultado

Nenhuma tabela crítica deve ser considerada confiável sem evidência de qualidade.

---

# Fase 7 — Incremental Processing / CDC

## Objetivo

Sair de pipelines simplistas de full refresh.

### Demonstrar

- watermark;
- incremental load;
- MERGE;
- CDC;
- late-arriving data;
- corrections;
- replay.

### Casos de teste

```text
Arquivo novo
Arquivo duplicado
Registro corrigido
Registro atrasado
Registro fora de ordem
```

---

# Fase 8 — Gold Data Products

## Objetivo

Criar datasets orientados ao negócio.

### Produtos

```text
Shipment Control Tower
Inventory in Transit
Route Performance
Carrier Performance
Returnables Balance
Operational Risk
```

### Principal

`gold.shipment_control_tower`

---

# Fase 9 — Risk Engine

## Objetivo

Identificar riscos antes que se transformem em problemas.

### Primeira versão

Rule-based.

Exemplos:

```text
ETA > Planned Delivery
```

```text
Destination Stock < Required Coverage
```

```text
GPS stale
```

```text
Vehicle stopped unexpectedly
```

### Resultado

```text
risk_score
risk_level
delay_probability
predicted_delay
impact_level
```

---

# Fase 10 — Incident Management

## Objetivo

Fechar o ciclo operacional.

```text
Alert
 ↓
Incident
 ↓
Owner
 ↓
Action
 ↓
Outcome
```

### Registrar

- owner;
- action;
- timestamp;
- status;
- resolution;
- outcome.

---

# Fase 11 — Orchestration

## Objetivo

Transformar pipelines isolados em workflows confiáveis.

### Demonstrar

- dependencies;
- scheduling;
- retries;
- timeout;
- failure;
- recovery;
- backfill;
- parameterization.

### Exemplo

```text
ERP ingestion ─────┐
TMS ingestion ─────┤
WMS ingestion ─────┤
GPS ingestion ─────┤
                   ▼
                 Bronze
                   ▼
                 Silver
                   ▼
                 Gold
                   ▼
              Risk Engine
                   ▼
                 Alerts
```

---

# Fase 12 — Observability

## Objetivo

Responder:

> O pipeline está funcionando?

> Os dados estão chegando?

> Os dados estão corretos?

> O negócio está sendo atendido?

### Camadas

#### Technical

- job status;
- duration;
- failures.

#### Data

- freshness;
- volume;
- quality.

#### Business

- delays;
- critical incidents;
- alerts;
- SLA.

---

# Fase 13 — Spark Performance Engineering

## Objetivo

Demonstrar domínio real de Spark.

### Experimentos

1. Logical Plan
2. Physical Plan
3. Catalyst
4. Stages
5. Tasks
6. Executors
7. Shuffle
8. Skew
9. Spill
10. Broadcast Join
11. Partitioning
12. Small Files
13. AQE
14. Caching

Cada experimento deverá possuir:

```text
Baseline
Optimization
Measurement
Result
Trade-off
```

---

# Fase 14 — Testing

## Unit

Testar funções puras.

## Integration

Testar pipeline entre componentes.

## Data Quality

Testar regras.

## Regression

Garantir que mudanças não quebram resultados anteriores.

### Ferramentas

- pytest;
- Ruff;
- pre-commit.

---

# Fase 15 — Git / CI

## Objetivo

Criar fluxo de engenharia profissional.

```text
feature branch
      ↓
commit
      ↓
push
      ↓
Pull Request
      ↓
CI
      ↓
tests
      ↓
lint
      ↓
validation
```

---

# Fase 16 — Databricks Declarative Automation Bundles

## Objetivo

Transformar o projeto Databricks em código versionável e implantável.

Utilizar:

```text
databricks.yml
```

para definir:

- jobs;
- pipelines;
- targets;
- configurações;
- recursos.

Declarative Automation Bundles são atualmente o mecanismo recomendado pelo Databricks para empacotar projetos, recursos, testes e deployment em workflows de CI/CD.

---

# Fase 17 — CD / Deployment

## Ambientes

```text
DEV
 ↓
TEST
 ↓
PROD
```

### Pipeline

```text
validate
 ↓
test
 ↓
bundle validate
 ↓
deploy
 ↓
integration test
```

No Free Edition:

> Demonstrar o conceito e executar o máximo possível no ambiente disponível, documentando claramente as limitações.

No ambiente corporativo:

> Implementar deployment automatizado completo.

---

# Fase 18 — Governance & Security

## Demonstrar

- Unity Catalog;
- RBAC;
- least privilege;
- ownership;
- lineage;
- audit;
- data classification.

### Modelo

```text
Catalog
  ↓
Schema
  ↓
Tables
  ↓
Permissions
```

---

# Fase 19 — Analytics / Power BI

## Objetivo

Disponibilizar Data Products.

### Dashboards

#### Control Tower

- transferências em risco;
- ETA;
- atrasos;
- criticidade;
- owner.

#### Logistics Performance

- carrier;
- route;
- SLA.

#### Returnables

- inventory;
- transfers;
- risk.

#### Data Quality

- freshness;
- failures;
- quality score.

---

# Fase 20 — ML opcional

Somente após o Data Engineering estar sólido.

### Problema

Prever atraso.

### Features possíveis

- route;
- carrier;
- historical delay;
- departure delay;
- GPS behavior;
- distance;
- hour;
- day;
- weather, se incluído.

### Resultado

Comparar:

```text
Rule-based
      vs
ML
```

O ML não deve ser utilizado apenas para "embelezar" o projeto.

---

# Fase 21 — Incident Simulation

Criar cenários controlados:

### Incident 1

Veículo parado.

### Incident 2

GPS stale.

### Incident 3

ETA atrasado.

### Incident 4

Destino próximo de ruptura.

### Incident 5

Fonte indisponível.

### Incident 6

Schema drift.

### Incident 7

Duplicação de arquivo.

### Incident 8

Late-arriving event.

O objetivo é provar que a arquitetura responde corretamente.

---

# Fase 22 — Recovery / Backfill

Simular:

```text
Pipeline failed
     ↓
Investigate
     ↓
Retry
     ↓
Recover
```

E:

```text
Historical correction
     ↓
Backfill
     ↓
Reprocess
     ↓
Validate
```

---

# Fase 23 — Documentation

Criar:

```text
README
Architecture
Data Dictionary
Data Contracts
ADRs
Runbooks
Incident Response
Deployment Guide
Testing Strategy
Performance Lab
```

---

# Fase 24 — Portfolio / Technical Presentation

Preparar apresentação:

1. Business Problem
2. Current State
3. Discovery
4. Architecture
5. Data Sources
6. Lakehouse
7. Data Quality
8. Spark
9. Risk Engine
10. Observability
11. CI/CD
12. Governance
13. Business Impact
14. Lessons Learned
15. Next Steps

---

# Fase 25 — Final Engineering Review

Checklist:

- [ ] Reproducible
- [ ] Tested
- [ ] Observable
- [ ] Documented
- [ ] Idempotent
- [ ] Incremental
- [ ] Governed
- [ ] Versioned
- [ ] Deployable
- [ ] Recoverable
- [ ] Performance tested
- [ ] Business validated

---

# Resultado final esperado

Ao final, o projeto deverá demonstrar:

**Business Understanding**

↓

**Data Architecture**

↓

**Data Engineering**

↓

**Spark**

↓

**Lakehouse**

↓

**Data Quality**

↓

**Observability**

↓

**DevOps**

↓

**Governance**

↓

**Data Product**

↓

**Business Value**
