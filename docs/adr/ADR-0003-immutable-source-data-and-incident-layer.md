# ADR-0003: Imutabilidade dos dados fonte e camada separada de incidentes

**Status:** Accepted
**Data:** Sprint 0
**Fase relacionada:** Fase 10 - Incident Management

## Context

O ciclo operacional (Alert → Incident → Owner → Action → Outcome) precisa registrar decisões humanas (ex: "ETA revisado de 18:00 para 14:30 porque o veículo foi substituído") sem contaminar o dado operacional original vindo do TMS/ERP. Se a ação sobrescrever o dado fonte, perdemos auditoria, rastreabilidade e a capacidade de medir se o alerta original estava correto (critério de sucesso "Prediction" da Sprint 0).

## Decision

1. Dados fonte (ERP, TMS, WMS, GPS, Assets) são **imutáveis** a partir do momento em que chegam à camada Bronze/Silver. Nenhuma ação operacional altera esses registros.
2. Toda ação operacional gera um **evento novo** em tabelas próprias, separadas dos dados operacionais:
   - `fact_risk_alert` — o alerta gerado pela Risk Engine, com o snapshot de risco no momento da detecção.
   - `fact_incident_event` — cada evento do ciclo de vida do incidente (assigned, investigating, action, resolved, validated), com owner, timestamp, ação e resultado.
3. Correções de negócio (ex: nova previsão de ETA) são armazenadas como um novo registro com `motivo`, `usuário` e `timestamp`, nunca como update destrutivo.

## Alternatives

- Update in-place no dado do TMS/ERP quando uma ação operacional muda a previsão: descartado — quebra auditabilidade e o critério de sucesso "Prediction" (não dá para avaliar se o alerta original acertou se o dado original foi sobrescrito).
- Log genérico não estruturado (texto livre) para ações: descartado — inviabiliza métricas de efetividade (Fase 12 - Observability, dimensão Business) e RBAC por tipo de ação.

## Consequences

- Exige um modelo de dados adicional (2 tabelas fato) além do modelo operacional — mais disciplina de modelagem, mas essencial para o Data Product responder "quem precisa agir" e "qual foi o resultado".
- Toda a Fase 10 (Incident Management) e Fase 12 (Observability, dimensão Business) dependem diretamente desta decisão.
- Reforça a decisão de RBAC (RACI, `docs/business/raci.md`): áreas registram ações no incidente, nunca no dado fonte.

## Status no ambiente de laboratório

`Implemented in lab` — schema Delta com as duas tabelas fato, versionadas junto com os data products em `pipelines/gold/`.
