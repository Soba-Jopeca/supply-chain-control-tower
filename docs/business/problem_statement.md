# Problem Statement — Supply Chain Returnables Control Tower

> Consolidado a partir da Sprint 0 (ver `docs/planning/02-sprint0-discovery.md` para o discovery completo).

## Problema

O problema central **não é ausência de dados**. É:

> Fragmentação dos dados + baixa integração + falta de contextualização + ausência de processo estruturado de resposta.

Isso mantém a operação em modo reativo: atrasos e rupturas de estoque de ativos retornáveis só são percebidos depois que já causaram impacto (fábrica sem material, estoque no limite, transportadora avisando problema, investigação manual).

## Visão de solução (hipótese validada em discovery)

Uma plataforma de dados (**Shipment Control Tower**) que combine ERP, TMS, WMS, GPS e Assets para responder, com antecedência mínima de 6h (premissa a validar):

> Quais transferências estão em risco, por que estão em risco, qual o impacto operacional, e quem precisa agir?

## Fluxo de valor do projeto

```
DADOS → INFORMAÇÃO → RISCO → DECISÃO → AÇÃO → RESULTADO → VALOR
```

## Critérios de sucesso (3 dimensões)

| Dimensão | Pergunta |
|---|---|
| Prediction | O alerta identificou corretamente o risco? |
| Lead Time | O alerta foi gerado com antecedência suficiente? |
| Operational Outcome | A informação permitiu uma ação que reduziu/evitou impacto? |

Princípio aceito: em situações críticas, é preferível um volume adicional de falsos positivos a perder atrasos críticos — mas excesso de alertas destrói confiança operacional. Por isso o risco é tratado por **criticidade**, não binário.

## Fora de escopo (nesta fase)

- Machine Learning para previsão de atraso (só depois da versão rule-based estar sólida e validada — Decisão 010 da Sprint 0).
- Streaming real-time (o negócio validou que atualização diária resolve 90% do problema; a arquitetura mantém a porta aberta para Event Hubs se o SLA mudar).
- Alteração/sobrescrita de dados fonte (imutabilidade é decisão de governança, não só técnica — ver ADR-0003).

## Referência cruzada

- Stakeholders e necessidades: `docs/business/stakeholders.md`
- Matriz RACI: `docs/business/raci.md`
- Perguntas em aberto: `docs/business/open_questions.md`
- Inventário de fontes: `docs/business/source_inventory.md`
- Decisões de arquitetura: `docs/adr/`
