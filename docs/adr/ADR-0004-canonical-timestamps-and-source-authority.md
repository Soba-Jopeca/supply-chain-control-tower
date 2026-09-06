# ADR-0004: Timestamps canônicos de atraso/chegada e autoridade de fonte em caso de conflito

**Status:** Accepted
**Data:** Fase 0 — fechamento de Open Questions (rodada 2 de Discovery)
**Fase relacionada:** Fase 1 - Solution Architecture / Fase 5 - Silver / Fase 9 - Risk Engine
**Resolve:** OQ-001, OQ-002, OQ-009

## Context

O discovery (Sprint 0, seção 6) identificou que Planning, Logística e Assets interpretam "atraso" de formas diferentes, e não existe uma definição oficial. Da mesma forma, não há uma fonte declarada como autoritativa quando ERP, TMS, WMS e GPS discordam sobre o estado de uma transferência (OQ-009). Sem resolver isso, a camada Silver não tem regra de reconciliação e a Risk Engine (Fase 9) não tem um sinal de entrada bem definido.

Tratar "atraso" como um único timestamp/flag esconde que ele serve a propósitos diferentes: Planning quer saber se o plano foi violado; a Risk Engine precisa de um sinal *antecipado* para gerar alerta; e a validação de efetividade (critério de sucesso "Prediction", Sprint 0 seção 12) precisa de um *ground truth* retroativo para saber se o alerta acertou.

## Decision

### 1. Três métricas de atraso, nomeadas e coexistentes (não uma "oficial" única)

| Métrica | Fórmula | Fontes | Propósito |
|---|---|---|---|
| `schedule_deviation` | horário atual vs. `ERP.planned_pickup` | ERP | Sinal de desvio de plano (visão de Planning) |
| `eta_risk` | `TMS.ETA` vs. `ERP.planned_delivery` | TMS + ERP | Sinal antecipado — **input direto da Risk Engine (Fase 9)** |
| `actual_delay` | `WMS.goods_receipt` vs. `ERP.planned_delivery` | WMS + ERP | Ground truth retroativo — usado para medir a dimensão "Prediction" do critério de sucesso |

Nenhuma substitui a outra. `eta_risk` é o gatilho de alerta; `actual_delay` é o que valida, depois do fato, se o alerta estava certo.

### 2. Evento de chegada efetiva

`WMS.goods_receipt` é o evento canônico de chegada efetiva. `TMS.status = delivered` é tratado como **proxy operacional** (confirma chegada do veículo ao pátio/doca), não como chegada de negócio — só o WMS confirma conferência física da carga. Todo cálculo de `actual_delay` e todo fechamento de incidente por "chegada" usa `WMS.goods_receipt`; nunca o status do TMS isoladamente.

### 3. Autoridade de fonte por entidade (regra de reconciliação do Silver)

| Entidade | Fonte autoritativa | Justificativa |
|---|---|---|
| Dados de planejamento (datas planejadas, quantidade, status do shipment) | ERP | Sistema de origem do plano |
| Eventos de transporte, ETA, status em rota | TMS | Especialista de domínio |
| Movimentação física (goods issue / goods receipt) | WMS | Única fonte com confirmação física |
| Localização / telemetria | GPS | Informativo apenas — **nunca** autoritativo sobre status de negócio |
| Saldo / disponibilidade de ativos | Sistema de Assets | Especialista de domínio |

GPS não sobrepõe status de negócio de nenhuma entidade — reforça o princípio já registrado no discovery (seção 8): ausência ou staleness de GPS não deve ser interpretada como ausência de risco, nem como confirmação de posição incorreta de outra fonte.

Em caso de conflito entre fontes sobre o mesmo campo de uma entidade não coberta pela tabela acima, a regra default é: **a fonte de origem do dado (system of record) vence; as demais são consideradas observações derivadas**, registradas mas não usadas para sobrescrever.

## Alternatives

- **Definir um único "delay_flag" binário calculado no ERP**: descartado — perde a granularidade entre desvio de plano, risco antecipado e atraso confirmado; impossibilita medir "Prediction" separadamente de "desvio percebido por Planning".
- **TMS como fonte autoritativa de tudo relacionado a transporte, incluindo chegada**: descartado — TMS não confirma recebimento físico, só movimentação do veículo; usar `status=delivered` como chegada geraria falsos "outcomes" (veículo chegou, carga não foi conferida/aceita).
- **Resolver conflitos por "last write wins" (timestamp mais recente vence)**: descartado — não é uma regra de negócio, é uma regra técnica genérica que ignora que cada fonte é especialista em domínios diferentes; abriria brecha para GPS ou fonte de baixa qualidade sobrescrever dado de sistema de origem.

## Consequences

- A camada Silver precisa implementar as três métricas como colunas derivadas explícitas (não um campo `is_delayed` genérico) — mais colunas, mas rastreável e testável isoladamente.
- A Risk Engine (Fase 9) consome `eta_risk` como sinal primário; `actual_delay` só existe depois do fato e alimenta a Fase 12 (Observability, dimensão Business) e a validação do critério de sucesso.
- A regra de autoridade por entidade vira insumo direto do módulo de reconciliação do Silver (Fase 5) — precisa de teste de integração dedicado simulando conflito entre fontes (ex: TMS diz "delivered", WMS não tem goods_receipt ainda).
- OQ-001, OQ-002 e OQ-009 passam de "Aberta" para "Assumida (lab) — ver ADR-0004" em `open_questions.md`.

## Status no ambiente de laboratório

`Implemented in lab` — as três métricas e a tabela de autoridade são regras determinísticas, sem dependência de recursos exclusivos de ambiente corporativo; implementáveis integralmente no Databricks Free Edition.
