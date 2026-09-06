# Design Notes

Decisões técnicas identificadas durante o design, deliberadamente adiadas
para a fase em que há informação suficiente (volume real, schema físico,
Data Contracts) para decidir sem chute. Cada nota vira ADR quando a
fase-gatilho for executada — nunca é implementada silenciosamente antes disso.

## DN-001 — Histórico de eta_risk (fact_eta_history)

**Identificada em:** Fase 1 (Context Diagram)
**Decidir em:** Fase 5 (Silver)
**Contexto:** `eta_risk` (ADR-0004) muda a cada atualização de TMS.ETA. Sem
histórico append-only, não é possível validar a dimensão "Lead Time" do
critério de sucesso (Sprint 0, seção 12) depois do fato.
**Opções em aberto:** (a) `eta_risk` mutável na Silver + `fact_eta_history`
separado; (b) tratar toda atualização de ETA como evento imutável desde a
ingestão (mais caro, mais alinhado ao ADR-0003).

## DN-002 — Cadência de ingestão e freshness por fonte

**Identificada em:** Fase 1 (Data Flow)
**Decidir em:** Fase 3 (Ingestion)
**Contexto:** SLAs divergentes por fonte (ERP ≤48h, TMS ≤2h, WMS ≤24h, GPS
best-effort — ADR-0004/OQ-010) exigem jobs de ingestão independentes por
fonte, não um job monolítico, e freshness como métrica por tabela (Fase 12),
não global.
**Dependência antecipada:** empurra conceitos de incremental/watermark
(Fase 7) para dentro da Fase 3 — TMS não sustenta full refresh a cada 2h.

## DN-003 — Contenção de SQL Warehouse único

**Identificada em:** Fase 1 (Deployment Architecture)
**Decidir em:** Fase 13 (Spark Performance Lab)
**Contexto:** Free Edition tem 1 SQL Warehouse 2X-Small e máx. 5 job tasks
concorrentes. Um MERGE pesado de Silver→Gold rodando ao mesmo tempo que uma
consulta do Power BI em `gold.shipment_control_tower` gera contenção real,
não apenas didática.
**Ação:** incluir "warehouse contention" como experimento dedicado no Spark
Performance Lab, além dos genéricos (broadcast join, skew, shuffle).
