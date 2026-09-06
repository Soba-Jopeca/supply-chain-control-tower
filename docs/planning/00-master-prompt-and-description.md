# Supply Chain Returnables Control Tower

## 1. Descrição do projeto

O **Supply Chain Returnables Control Tower** é um projeto de Data Engineering orientado a um cenário realista de Supply Chain e Logistics, cujo objetivo é construir um Data Product capaz de integrar dados de transferências intercompany, transporte, WMS, GPS, estoque de ativos retornáveis e ocorrências operacionais.

O projeto deve simular um ambiente corporativo moderno no qual dados provenientes de diferentes sistemas possuem diferentes formatos, frequências, níveis de qualidade e regras de negócio.

O objetivo não é construir apenas um dashboard.

O objetivo é construir uma plataforma de dados capaz de transformar:

**Dados → Informação → Risco → Decisão → Ação → Resultado → Feedback**

O principal Data Product será o **Shipment Control Tower**, responsável por responder:

> Quais transferências estão em risco neste momento, por que estão em risco, qual o impacto operacional, quem precisa agir e qual foi o resultado da ação?

---

## 2. Contexto de negócio

A organização possui transferências de materiais e ativos entre diferentes unidades.

As informações estão distribuídas entre:

- ERP/SAP;
- TMS;
- WMS;
- GPS/telemetria;
- sistemas de ativos;
- planilhas;
- registros manuais.

Os dados existem, porém são fragmentados.

A consequência é que situações críticas podem ser identificadas apenas quando:

- a fábrica percebe que o material não chegou;
- o estoque está próximo de ruptura;
- a transportadora informa um problema;
- alguém realiza uma investigação manual.

O projeto pretende transformar esse processo reativo em um processo preventivo.

---

## 3. Problema de negócio

A ausência de uma visão integrada e antecipada do ciclo de vida das transferências dificulta:

- identificar atrasos antecipadamente;
- avaliar o impacto operacional;
- priorizar situações críticas;
- definir ownership;
- acionar as áreas responsáveis;
- acompanhar a resolução;
- medir a efetividade das ações.

O projeto deverá demonstrar como uma plataforma moderna de dados pode solucionar esse problema.

---

# 4. Objetivos técnicos

O projeto deve demonstrar domínio prático de:

### Data Engineering

- Python;
- PySpark;
- Spark SQL;
- DataFrames;
- Spark transformations/actions;
- joins;
- aggregations;
- window functions;
- partitioning;
- caching;
- broadcast joins;
- shuffle;
- skew;
- spill;
- small files;
- execution plans;
- Catalyst;
- stages;
- tasks;
- executors.

### Lakehouse

- Data Lake;
- Lakehouse;
- Delta Lake;
- ACID;
- schema enforcement;
- schema evolution;
- MERGE;
- incremental processing;
- watermark;
- CDC;
- late-arriving data;
- idempotency;
- backfill;
- replay/reprocessing.

### Arquitetura

Implementar uma arquitetura Medallion:

```text
Bronze
   ↓
Silver
   ↓
Gold
```

A arquitetura Medallion deverá ser utilizada como padrão de organização lógica e evolução da qualidade dos dados, seguindo a orientação do Databricks para construção de fontes confiáveis e Data Products.

### Data Quality

- schema validation;
- data contracts;
- uniqueness;
- null checks;
- referential integrity;
- business rules;
- duplicate detection;
- quarantine;
- quality metrics;
- freshness;
- completeness;
- validity;
- consistency.

Quando aplicável, utilizar mecanismos nativos do Databricks/Lakeflow para expectativas de qualidade, mantendo as regras de qualidade separadas da lógica principal para favorecer reutilização e manutenção.

### Orchestration

Demonstrar:

- dependencies;
- retries;
- failure handling;
- backfill;
- scheduling;
- parameterization;
- recovery;
- monitoring.

### DevOps

Utilizar:

- Git;
- GitHub;
- branches;
- pull requests;
- code review;
- conventional commits;
- unit tests;
- integration tests;
- linting;
- formatting;
- pre-commit;
- CI/CD.

Para Databricks, utilizar **Declarative Automation Bundles**, atualmente recomendados pela plataforma para estruturar projetos com source control, testes, jobs, pipelines e CI/CD.

### IaC

Quando tecnicamente viável:

- Terraform;
- Databricks resources;
- environment configuration;
- deployment targets.

### Governance

Demonstrar conceitos de:

- Unity Catalog;
- catalog;
- schema;
- table ownership;
- RBAC;
- least privilege;
- lineage;
- auditability.

### Analytics

Utilizar:

- Databricks SQL;
- Power BI.

---

# 5. Arquitetura alvo

A arquitetura conceitual deverá seguir:

```text
ERP / SAP
     │
TMS ─┤
WMS ─┤
GPS ─┤
ASSETS┤
Files ┘
     │
     ▼
Landing / Raw
     │
     ▼
Azure Data Lake Storage Gen2
     │
     ▼
Azure Databricks
     │
     ├── Bronze
     │
     ├── Silver
     │
     └── Gold
             │
             ├── Shipment Control Tower
             ├── Inventory in Transit
             ├── Route Performance
             ├── Carrier Performance
             ├── Returnables Balance
             └── Operational Risk
                     │
                     ▼
              Databricks SQL
                     │
                     ▼
                  Power BI
```

Na arquitetura corporativa alvo, considerar serviços Azure existentes e adequados ao cenário, como:

- Azure Data Factory;
- ADLS Gen2;
- Azure Databricks;
- Azure Key Vault;
- Azure Monitor;
- Azure DevOps ou GitHub;
- Power BI;
- Azure Event Hubs, caso streaming seja necessário.

As arquiteturas de referência do Azure Databricks utilizam justamente padrões envolvendo ADF para batch, ADLS Gen2 como armazenamento, Event Hubs para streaming e Power BI para BI, entre outros serviços.

---

# 6. Ambiente de laboratório

O projeto deve distinguir explicitamente:

### Arquitetura alvo

Azure + Databricks corporativo.

### Implementação de laboratório

Databricks Free Edition + dados sintéticos.

Não simular capacidades inexistentes no Free Edition.

Quando uma funcionalidade exigir recursos de ambiente corporativo, documentar:

```text
Implemented in lab
```

ou:

```text
Target production implementation
```

ou:

```text
Demonstrated conceptually
```

---

# 7. Fontes de dados sintéticas

Criar geradores independentes para:

### ERP

Campos:

- shipment_id;
- created_at;
- origin_plant;
- destination_plant;
- material_id;
- quantity;
- uom;
- planned_pickup;
- planned_delivery;
- status;
- updated_at.

### TMS

- shipment_id;
- carrier_id;
- vehicle_id;
- route_id;
- planned_departure;
- actual_departure;
- ETA;
- status;
- updated_at.

### WMS

- shipment_id;
- warehouse;
- dock;
- loading_start;
- loading_end;
- goods_issue;
- goods_receipt.

### GPS

- vehicle_id;
- timestamp;
- latitude;
- longitude;
- speed;
- heading.

### Assets

- asset_id;
- asset_type;
- location;
- available_quantity;
- reserved_quantity;
- consumption_rate.

### Master Data

- plants;
- materials;
- routes;
- carriers;
- vehicles.

---

# 8. Dados propositalmente imperfeitos

Os dados devem simular problemas reais.

Introduzir:

- duplicidades;
- nulls;
- IDs inconsistentes;
- nomes diferentes para a mesma entidade;
- formatos diferentes de data;
- registros atrasados;
- eventos fora de ordem;
- arquivos duplicados;
- registros corrigidos;
- cancelamentos;
- recriação de transferências;
- schema drift;
- GPS stale;
- chaves ausentes;
- quantidades divergentes;
- eventos faltantes.

O objetivo é que o pipeline tenha problemas reais para resolver.

Não criar um dataset artificialmente perfeito.

---

# 9. Volumetria inicial

Criar aproximadamente:

- 24 meses;
- 50.000 shipments;
- 200.000 shipment items;
- 300.000 transport events;
- 1.000.000 GPS events;
- 12 plants;
- 30 materials;
- 40 carriers;
- 150 routes;
- 600 vehicles.

A volumetria deverá ser configurável.

Criar parâmetros para permitir:

```text
small
medium
large
stress
```

Isso permitirá estudar performance do Spark.

---

# 10. Data Products

Criar inicialmente:

### Gold — Shipment Control Tower

Principal Data Product.

Deve responder:

- onde está a transferência;
- quando deveria chegar;
- quando provavelmente chegará;
- qual o atraso previsto;
- qual o risco;
- qual o impacto;
- quem deve agir.

### Gold — Inventory in Transit

Visão dos ativos/materiais em trânsito.

### Gold — Route Performance

Performance das rotas.

### Gold — Carrier Performance

Performance das transportadoras.

### Gold — Returnables Balance

Saldo e movimentação dos ativos.

### Gold — Operational Risk

Visão consolidada de risco.

---

# 11. Risk Engine

A primeira versão deve ser baseada em regras explicáveis.

Não iniciar diretamente com Machine Learning.

Considerar:

- ETA;
- planned delivery;
- delay;
- destination stock;
- consumption;
- asset criticality;
- GPS freshness;
- vehicle status;
- route characteristics;
- data confidence.

Produzir:

```text
risk_score
risk_level
predicted_delay_hours
operational_impact
alert_required
```

Depois que a solução determinística estiver funcionando, pode ser criado um módulo opcional de ML para previsão de atraso.

---

# 12. Incident Management

O projeto deve permitir representar:

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

Registrar:

- alert_id;
- shipment_id;
- created_at;
- risk_level;
- owner_area;
- owner;
- status;
- acknowledged_at;
- action_at;
- action_type;
- action_description;
- resolved_at;
- actual_delay;
- operational_impact;
- outcome.

O dado operacional original não deve ser sobrescrito.

As ações devem gerar eventos adicionais.

---

# 13. Data Quality

Implementar uma camada estruturada de qualidade.

Exemplos:

```text
shipment_id IS NOT NULL
shipment_id UNIQUE
quantity > 0
origin != destination
status IN allowed_values
updated_at IS NOT NULL
```

Criar:

```text
valid_records
invalid_records
quarantine_records
quality_metrics
```

Produzir métricas de:

- completeness;
- validity;
- uniqueness;
- consistency;
- freshness.

---

# 14. Observability

Criar observabilidade em:

### Pipeline

- execution_id;
- start_time;
- end_time;
- duration;
- status;
- records_read;
- records_written;
- records_rejected;
- error_message.

### Data

- freshness;
- row count;
- null percentage;
- duplicate percentage;
- quality score.

### Business

- alerts;
- critical incidents;
- unresolved incidents;
- SLA violations;
- avoided impacts.

---

# 15. Spark Performance Lab

O projeto deve conter experimentos documentados.

Demonstrar:

### Experiment 1

Normal join vs broadcast join.

### Experiment 2

Data skew.

### Experiment 3

Shuffle.

### Experiment 4

Small files.

### Experiment 5

Partitioning.

### Experiment 6

Caching.

### Experiment 7

Logical vs physical plan.

### Experiment 8

AQE.

Para cada experimento documentar:

```text
Problem
Hypothesis
Baseline
Optimization
Evidence
Result
Trade-off
Conclusion
```

Não aceitar otimizações sem evidência.

Utilizar:

```text
explain()
Spark UI
query metrics
execution plan
```

---

# 16. Estrutura esperada do projeto

```text
supply-chain-control-tower/

├── README.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── LICENSE
│
├── databricks.yml
├── pyproject.toml
├── .gitignore
├── .pre-commit-config.yaml
│
├── config/
│   ├── dev.yml
│   ├── test.yml
│   └── prod.yml
│
├── src/
│   ├── ingestion/
│   ├── transformations/
│   ├── quality/
│   ├── monitoring/
│   ├── risk/
│   └── utils/
│
├── pipelines/
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── data_generator/
│   ├── erp/
│   ├── tms/
│   ├── wms/
│   ├── gps/
│   ├── assets/
│   └── master_data/
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── data_quality/
│
├── resources/
│   ├── jobs.yml
│   └── pipelines.yml
│
├── infrastructure/
│   └── terraform/
│
├── docs/
│   ├── architecture/
│   ├── business/
│   ├── data_model/
│   ├── adr/
│   ├── runbooks/
│   └── performance/
│
└── .github/
    └── workflows/
        ├── ci.yml
        └── deploy.yml
```

---

# 17. Engenharia de software

Aplicar:

- PEP 8;
- type hints;
- docstrings quando agregarem valor;
- funções pequenas;
- módulos coesos;
- tratamento explícito de erros;
- logging estruturado;
- configuração externa;
- secrets fora do código;
- testes automatizados.

Utilizar:

- pytest;
- Ruff;
- pre-commit;
- Git;
- GitHub Actions.

---

# 18. Regras para Claude Code

Você deve atuar como:

**Senior Data Engineer + Data Architect + Code Reviewer + Technical Mentor.**

Não aja como um gerador de código cego.

Antes de implementar uma funcionalidade:

1. entenda o requisito;
2. identifique dependências;
3. verifique o estado atual do projeto;
4. proponha abordagem;
5. identifique riscos;
6. implemente;
7. teste;
8. valide;
9. documente;
10. informe trade-offs.

Não criar código desnecessário.

Não duplicar lógica.

Não criar notebooks gigantes com toda a aplicação.

Priorizar módulos Python reutilizáveis e notebooks finos para exploração/orquestração.

---

# 19. Regra fundamental

Nunca alterar uma decisão de arquitetura importante silenciosamente.

Quando houver uma decisão relevante, criar um ADR:

```text
docs/adr/ADR-XXXX-title.md
```

Estrutura:

```text
Context
Decision
Alternatives
Consequences
Status
```

---

# 20. Definition of Done

Uma funcionalidade somente será considerada concluída quando:

- código implementado;
- testes criados;
- testes executados;
- qualidade de código validada;
- documentação atualizada;
- observabilidade considerada;
- impacto de performance avaliado quando aplicável;
- segurança considerada;
- idempotência considerada;
- tratamento de erro implementado;
- configuração externalizada;
- PR-ready.

---

# 21. Princípio final

O projeto deve parecer um sistema que poderia existir em uma empresa real.

Evitar:

- datasets perfeitos;
- pipelines artificiais;
- notebooks monolíticos;
- código sem testes;
- regras de negócio escondidas;
- hardcode;
- dashboards desconectados do processo;
- ML sem necessidade;
- arquitetura complexa apenas para impressionar.

Priorizar:

**clareza → confiabilidade → qualidade → observabilidade → performance → escalabilidade → governança.**

A solução deve demonstrar não apenas que o desenvolvedor sabe utilizar Databricks, Spark e Azure, mas que sabe **resolver problemas de negócio utilizando engenharia de dados profissional.**

# Referências arquiteturais

Utilizar como referências oficiais:

- Azure Databricks Lakehouse;
- Databricks Medallion Architecture;
- Delta Lake;
- Databricks Well-Architected Framework;
- Unity Catalog;
- Lakeflow;
- Declarative Automation Bundles;
- GitHub Actions;
- Azure Data Factory;
- ADLS Gen2;
- Power BI;
- Terraform.

Sempre preferir documentação oficial e padrões atuais da plataforma em vez de padrões antigos ou deprecated.
