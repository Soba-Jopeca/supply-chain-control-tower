# Sprint 0 — Discovery

## Supply Chain Returnables Control Tower

**Status:** Discovery inicial concluído
**Objetivo:** Entender o problema de negócio antes da definição da solução técnica.

---

# 1. Objetivo da Sprint 0

A Sprint 0 teve como objetivo compreender:

- problema atual;
- impacto operacional;
- stakeholders;
- processo atual;
- fontes de informação;
- decisões tomadas;
- dificuldades;
- critérios de sucesso;
- ownership;
- governança;
- necessidades futuras.

O objetivo não foi definir antecipadamente tecnologias ou pipelines.

A abordagem utilizada foi:

```text
Problema
 ↓
Processo atual
 ↓
Decisões
 ↓
Impacto
 ↓
Dados necessários
 ↓
Requisitos
 ↓
Solução
```

---

# 2. Stakeholders

## Mariana — Planning Manager

Responsabilidades:

- planejamento;
- priorização;
- programação;
- avaliação de necessidade;
- decisão operacional.

Principais necessidades:

- antecipar problemas;
- avaliar impacto;
- priorizar transferências;
- tomar decisões preventivas.

---

## Rafael — Logistics Coordinator

Responsabilidades:

- transporte;
- transportadoras;
- veículos;
- rotas;
- acompanhamento operacional.

Principais necessidades:

- visibilidade da viagem;
- ETA;
- localização;
- ocorrência;
- capacidade de reação.

---

## Camila — Returnable Assets Manager

Responsabilidades:

- disponibilidade de ativos;
- estoque;
- risco de ruptura;
- movimentação dos ativos.

Principais necessidades:

- entender impacto dos atrasos;
- identificar risco de ruptura;
- priorizar ativos críticos.

---

## André — Data Platform Architect

Responsabilidades:

- arquitetura;
- plataforma;
- governança;
- engenharia;
- integração.

Principais necessidades:

- confiabilidade;
- escalabilidade;
- observabilidade;
- governança;
- segurança;
- manutenção.

---

# 3. Current State

Atualmente os dados necessários estão distribuídos entre:

```text
ERP
TMS
WMS
GPS
Assets
Planilhas
E-mails
Teams
Registros manuais
```

Não existe necessariamente uma visão consolidada do ciclo:

```text
Programação
 ↓
Transporte
 ↓
Localização
 ↓
ETA
 ↓
Chegada
 ↓
Impacto
 ↓
Ação
 ↓
Resultado
```

---

# 4. Problema identificado

O problema central não é ausência de dados.

É:

> **fragmentação dos dados + baixa integração + falta de contextualização + ausência de processo estruturado de resposta.**

Isso gera atuação predominantemente reativa.

---

# 5. Situação atual de detecção

Uma fábrica pode perceber que uma transferência está atrasada quando:

- a data programada foi ultrapassada;
- o material não chegou;
- o estoque está próximo de ruptura;
- alguém consulta manualmente o TMS;
- ocorre uma comunicação da operação.

Não existe necessariamente uma visão única que responda:

> Quais transferências de hoje estão em risco?

---

# 6. Divergência conceitual sobre atraso

Foi identificado que as áreas podem interpretar atraso de maneiras diferentes.

Exemplo:

```text
Programado: 08:00
ETA TMS: 14:00
Horário atual: 10:00
```

Planning pode interpretar:

> existe desvio em relação ao planejamento.

Logística pode interpretar:

> o transporte ainda pode chegar dentro de uma janela operacional aceitável.

Assets pode interpretar:

> o impacto depende do estoque disponível no destino.

Portanto:

**A definição oficial de atraso ainda precisa ser validada.**

---

# 7. Fatores de atraso identificados

Possíveis fatores:

- atraso no carregamento;
- indisponibilidade de veículo;
- atraso de chegada do veículo;
- documentação;
- capacidade da transportadora;
- trânsito;
- interrupções de rota;
- problemas na rota;
- espera no destino;
- reprogramação;
- problemas operacionais.

A classificação de causa ainda não está necessariamente estruturada.

---

# 8. Visibilidade atual

O TMS possui informações como:

- viagem;
- veículo;
- transportadora;
- origem;
- destino;
- status;
- ETA;
- alguns eventos.

GPS pode existir, mas:

- nem todos os veículos possuem cobertura;
- pode haver gaps;
- a frequência pode variar;
- os dados podem ficar desatualizados.

Consequentemente:

> ausência de GPS não deve automaticamente significar ausência de risco.

---

# 9. Insight central

A solução não deve depender de uma única fonte.

Precisamos combinar:

```text
TMS
+
GPS
+
ERP
+
WMS
+
Assets
+
Contexto operacional
```

---

# 10. Antecipação

Foi proposta a capacidade de identificar risco de atraso com pelo menos:

**6 horas de antecedência.**

Essa premissa deverá ser validada.

O sistema deverá posteriormente definir:

- referência temporal;
- janela de previsão;
- tolerância;
- SLA;
- nível de confiança.

---

# 11. Valor esperado

Se o risco for identificado antecipadamente, Planning poderá:

- revisar estoque;
- priorizar;
- reprogramar;
- buscar alternativa;
- acionar outras unidades.

Logística poderá:

- acionar transportadora;
- investigar veículo;
- buscar alternativa;
- acompanhar rota.

Assets poderá:

- avaliar risco de ruptura;
- identificar ativos críticos;
- priorizar reposição.

---

# 12. Critério de sucesso

O sucesso não será definido apenas pela previsão correta do atraso.

Existem três dimensões:

## Prediction

O alerta identificou corretamente o risco?

## Lead Time

O alerta foi gerado com antecedência suficiente?

## Operational Outcome

A informação permitiu uma ação que reduziu ou evitou impacto?

---

# 13. Falsos positivos e falsos negativos

Foi definido como princípio:

> Em situações críticas, é preferível aceitar algum volume adicional de falsos positivos a perder atrasos críticos.

Porém:

> excesso de alertas reduz confiança e capacidade operacional.

Portanto, o sistema deve trabalhar com criticidade.

---

# 14. Volume de alertas

Foi estimado inicialmente:

**10–20 alertas prioritários/dia**

como volume operacionalmente administrável.

Isso não significa que o sistema só possa detectar 20 eventos.

A arquitetura deverá separar:

```text
Events
 ↓
Risk Signals
 ↓
Alerts
 ↓
Prioritized Incidents
```

---

# 15. Definição de incidente

Um incidente é uma situação que:

- possui risco operacional relevante;
- requer avaliação;
- pode exigir ação;
- deve possuir owner quando crítico.

---

# 16. Ciclo de vida

```text
DETECTED
   ↓
ALERTED
   ↓
ASSIGNED
   ↓
INVESTIGATING
   ↓
ACTION / NO ACTION
   ↓
MONITORING
   ↓
RESOLVED
   ↓
VALIDATED
```

---

# 17. Definição de "tratamento"

Tratar um alerta significa:

1. receber;
2. analisar;
3. avaliar impacto;
4. tomar decisão;
5. registrar;
6. executar ação quando necessário;
7. acompanhar;
8. resolver;
9. validar resultado.

---

# 18. Ownership

Atualmente não existe necessariamente ownership formal para todos os incidentes.

Isso cria risco de:

- transferência de responsabilidade;
- demora;
- falta de acompanhamento;
- problemas sem resolução clara.

Foi proposta uma matriz inicial:

| Tipo | Owner |
|---|---|
| Transporte | Logística |
| Ativos | Assets |
| Reprogramação | Planning |
| Data Quality | Source/Data Owner |
| Impacto crítico | Planning |

Necessita validação.

---

# 19. Fonte de registro de incidentes

Atualmente as ações podem estar distribuídas entre:

- TMS;
- e-mails;
- Teams;
- planilhas;
- observações;
- mensagens;
- registros manuais.

Não existe uma fonte única estruturada para o ciclo:

```text
alerta
→ owner
→ ação
→ resultado
```

---

# 20. Recomendação

Criar uma camada estruturada de incidentes.

Exemplo:

```text
fact_risk_alert
fact_incident_event
```

Separadas dos dados operacionais originais.

---

# 21. Governança

Foi definido que as áreas não devem possuir os mesmos privilégios.

### Planning

Pode:

- visualizar;
- assumir;
- reatribuir;
- registrar;
- encerrar conforme domínio.

### Logística

Pode:

- visualizar;
- assumir transporte;
- registrar ações;
- reatribuir dentro das regras.

### Assets

Pode:

- visualizar;
- assumir incidentes de ativos;
- registrar impacto;
- acompanhar.

### Fábrica

Pode:

- visualizar casos relacionados;
- informar ocorrências;
- confirmar resolução.

Não deve alterar dados fonte.

---

# 22. Imutabilidade dos dados fonte

Decisão:

> A ação operacional não deve sobrescrever o dado original.

Exemplo:

```text
ETA original = 18:00

Nova previsão = 14:30

Motivo = veículo substituído

Usuário = Logística

Timestamp = 11:42
```

Isso garante:

- auditoria;
- rastreabilidade;
- histórico;
- confiabilidade.

---

# 23. Fontes de dados

## ERP

Transferências e planejamento.

## TMS

Transporte e ETA.

## WMS

Movimentações físicas.

## GPS

Localização e telemetria.

## Assets

Disponibilidade.

## Manual

Ocorrências e ajustes.

---

# 24. Requisitos funcionais

| ID | Requisito |
|---|---|
| FR-001 | Integrar ERP |
| FR-002 | Integrar TMS |
| FR-003 | Integrar WMS |
| FR-004 | Integrar Assets |
| FR-005 | Integrar GPS |
| FR-006 | Reconciliar fontes |
| FR-007 | Monitorar transferências |
| FR-008 | Detectar risco |
| FR-009 | Avaliar impacto |
| FR-010 | Priorizar alertas |
| FR-011 | Criar incidentes |
| FR-012 | Atribuir owner |
| FR-013 | Registrar ações |
| FR-014 | Registrar resultado |
| FR-015 | Auditar mudanças |
| FR-016 | Medir efetividade |

---

# 25. Requisitos não funcionais

### Reliability

Sistema confiável.

### Idempotency

Reprocessamentos não podem gerar duplicidade.

### Scalability

Suportar crescimento.

### Freshness

Dados devem obedecer SLA.

### Observability

Pipelines e dados devem ser observáveis.

### Recoverability

Falhas devem permitir retry/recovery.

### Auditability

Ações devem ser rastreáveis.

### Security

Acesso baseado em função.

### Maintainability

Código modular e testável.

---

# 26. Open Questions

## OQ-001

Qual timestamp define oficialmente atraso?

## OQ-002

Qual evento representa chegada efetiva?

## OQ-003

Como calcular consumo/necessidade?

## OQ-004

Qual estoque define risco?

## OQ-005

Quais ativos são críticos?

## OQ-006

A janela de 6 horas é adequada para todos os cenários?

## OQ-007

Quem é owner de cada incidente?

## OQ-008

Quem pode encerrar?

## OQ-009

Qual fonte é authoritative em caso de conflito?

## OQ-010

Qual SLA de atualização por fonte?

## OQ-011

Qual volume máximo de alertas?

## OQ-012

Como validar alertas que evitaram atrasos?

---

# 27. Hipótese do Data Product

## Shipment Control Tower

O Data Product deverá responder:

> **Quais transferências estão em risco, qual o impacto, por que estão em risco e quem precisa agir?**

---

# 28. Arquitetura conceitual inicial

```text
             DATA SOURCES
                  │
                  ▼
               LANDING
                  │
                  ▼
                BRONZE
                  │
                  ▼
                SILVER
                  │
                  ▼
                 GOLD
                  │
       ┌──────────┴──────────┐
       ▼                     ▼
 Control Tower          Data Products
       │
       ▼
 Risk Detection
       │
       ▼
 Alerts
       │
       ▼
 Incidents
       │
       ▼
 Actions
       │
       ▼
 Outcomes
```

---

# 29. Decisões de Sprint 0

### Decisão 001

O projeto será orientado a Data Product, não apenas BI.

### Decisão 002

Será utilizada arquitetura Lakehouse.

### Decisão 003

A arquitetura lógica seguirá Bronze/Silver/Gold.

### Decisão 004

Dados fonte serão preservados.

### Decisão 005

Incidentes terão histórico próprio.

### Decisão 006

Ownership será explícito.

### Decisão 007

Risco será contextual, não apenas baseado em atraso.

### Decisão 008

Riscos críticos terão maior prioridade de recall.

### Decisão 009

Alertas serão avaliados pelo resultado operacional.

### Decisão 010

ML será posterior à implementação determinística.

---

# 30. Definition of Done da Sprint 0

A Sprint 0 será considerada concluída quando:

- [x] Problema identificado;
- [x] Stakeholders identificados;
- [x] Current State documentado;
- [x] Necessidade definida;
- [x] Critérios iniciais de sucesso definidos;
- [x] Fontes identificadas;
- [x] Ownership discutido;
- [x] Governança inicial definida;
- [x] Requisitos iniciais documentados;
- [x] Open Questions registradas;
- [x] Arquitetura conceitual criada;
- [x] Backlog inicial criado.

---

# 31. Próxima etapa

A próxima etapa será uma segunda rodada de Discovery focada nas Open Questions.

Especialmente:

1. definição oficial de atraso;
2. definição de SLA;
3. regras de estoque;
4. criticidade dos ativos;
5. authoritative source;
6. ownership;
7. thresholds;
8. frequência das fontes;
9. Data Contracts;
10. regras de qualidade.

Somente após essas definições a arquitetura física deverá ser detalhada.

---

# 32. Princípio orientador

O projeto será guiado pelo seguinte fluxo:

**DATA**

↓

**INFORMATION**

↓

**RISK**

↓

**DECISION**

↓

**ACTION**

↓

**OUTCOME**

↓

**VALUE**

O sucesso da plataforma será medido não apenas pela capacidade de processar dados, mas pela capacidade de transformar dados confiáveis em decisões operacionais melhores.
