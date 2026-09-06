# RACI — Gestão de Incidentes

Matriz inicial (proposta na Sprint 0, seção 18/21 — **necessita validação**, ver OQ-007/OQ-008).

| Tipo de incidente | Responsible | Accountable | Consulted | Informed |
|---|---|---|---|---|
| Transporte (atraso, veículo) | Logística | Logistics Coordinator | Carrier | Planning |
| Ativos (ruptura, criticidade) | Assets | Returnable Assets Manager | Planning | Fábrica |
| Reprogramação | Planning | Planning Manager | Logística, Assets | Fábrica |
| Data Quality (fonte com problema) | Source/Data Owner | Data Platform Architect | Time de engenharia | Todas as áreas consumidoras |
| Impacto crítico (ruptura iminente) | Planning | Planning Manager | Assets, Logística | Fábrica, Diretoria |

## Regras de governança por área (permissões, não RACI)

| Área | Pode |
|---|---|
| Planning | visualizar, assumir, reatribuir, registrar, encerrar conforme domínio |
| Logística | visualizar, assumir transporte, registrar ações, reatribuir dentro das regras |
| Assets | visualizar, assumir incidentes de ativos, registrar impacto, acompanhar |
| Fábrica | visualizar casos relacionados, informar ocorrências, confirmar resolução — **não altera dados fonte** |

Este modelo de permissões é o input direto para o desenho de RBAC no Unity Catalog (Fase 18 / ADR de governança).
