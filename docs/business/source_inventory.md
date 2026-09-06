# Source Inventory

| Fonte | Sistema simulado | Entidades principais | Frequência (lab) | Qualidade esperada |
|---|---|---|---|---|
| ERP | SAP | shipments, shipment_items | Diária, com atraso de 1-2 dias | Boa, mas atrasada |
| TMS | Transport Mgmt System | transport_events, ETA, status | Múltiplas vezes ao dia | Média — eventos fora de ordem |
| WMS | Warehouse Mgmt System | goods_issue, goods_receipt, docas | Diária/por evento | Média |
| GPS | Telemetria de veículo | localização, velocidade | Intervalos variáveis, gaps | Baixa/inconsistente — cobertura parcial |
| Assets | Sistema de ativos retornáveis | saldo, disponibilidade, criticidade | Diária | Média |
| Master Data | Cadastros | plants, materials, routes, carriers, vehicles | Baixa frequência (quase estática) | Boa, mas com drift ocasional |
| Manual | Planilhas/e-mail/Teams | ocorrências, ajustes, ações de incidente | Ad hoc | Baixa — não estruturado |

Ver os data contracts detalhados por fonte (schema, campos, tipos) na Fase 2 (`data_generator/`) — cada gerador terá seu contrato versionado junto ao schema.
