# src/risk

Risk Engine — primeira versão rule-based (não ML). Calcula `risk_score`, `risk_level`, `predicted_delay_hours`, `operational_impact`, `alert_required` a partir de ETA, planned delivery, destination stock, consumption, asset criticality, GPS freshness, vehicle status.

Fase: 9 (Risk Engine). Ver ADR-0003 para o modelo de eventos de incidente que consome a saída deste módulo.
