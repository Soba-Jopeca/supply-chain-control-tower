# Open Questions

Rastreamento vivo. Atualizar status conforme forem respondidas (na Fase 0 / rodada 2 de Discovery). Nenhuma OQ crítica deve ficar em aberto antes de detalhar a arquitetura física (Fase 1).

| ID | Pergunta | Impacta | Status |
|---|---|---|---|
| OQ-001 | Qual timestamp define oficialmente atraso? | Risk Engine, Silver | Aberta |
| OQ-002 | Qual evento representa chegada efetiva? | Gold — Shipment Control Tower | Aberta |
| OQ-003 | Como calcular consumo/necessidade de ativos? | Returnables Balance | Aberta |
| OQ-004 | Qual estoque define risco de ruptura? | Risk Engine | Aberta |
| OQ-005 | Quais ativos são críticos? | Risk Engine, priorização | Aberta |
| OQ-006 | A janela de 6h é adequada para todos os cenários? | Risk Engine, SLA | Aberta |
| OQ-007 | Quem é owner de cada incidente? | RACI, RBAC | Aberta |
| OQ-008 | Quem pode encerrar um incidente? | RBAC, Incident Management | Aberta |
| OQ-009 | Qual fonte é authoritative em caso de conflito? | Silver — regras de reconciliação | Aberta |
| OQ-010 | Qual SLA de atualização por fonte? | Data Contracts, Freshness | Aberta |
| OQ-011 | Qual volume máximo de alertas administrável? | Risk Engine — thresholds | Aberta |
| OQ-012 | Como validar alertas que evitaram atrasos (contrafactual)? | Critério de sucesso / Outcome | Aberta |

## Decisão de projeto sobre as OQs

Como este é um projeto de portfólio (sem stakeholders reais para responder), cada OQ crítica será **assumida explicitamente como premissa documentada** (não silenciosa) via ADR ou nota em `docs/business/`, e marcada como `Assumida (lab)` — nunca resolvida "no código" sem registro.
