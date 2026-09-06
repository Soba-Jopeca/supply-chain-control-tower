# Changelog

Formato baseado em [Keep a Changelog](https://keepachangelog.com/).

## [Unreleased]

### Added
- Estrutura inicial do repositório e organização do projeto.
- Discovery (Sprint 0): problema de negócio, stakeholders, RACI, requisitos funcionais/não funcionais, open questions.
- ADR-0001: arquitetura Lakehouse Medallion.
- ADR-0002: separação lab vs. arquitetura alvo.
- ADR-0003: imutabilidade de dados fonte e camada de incidentes.
- Roadmap de 25 fases documentado.
- ADR-0004: timestamps canônicos de atraso/chegada (`schedule_deviation`, `eta_risk`, `actual_delay`) e tabela de autoridade de fonte por entidade em caso de conflito.

### Changed
- `open_questions.md`: OQ-001, OQ-002, OQ-009 fechadas como "Assumida (lab)" via ADR-0004; OQ-003 a OQ-006 movidas para "Deferida — Fase 9"; OQ-012 marcada como "Limitação reconhecida" (ver ADR-0002).

### Open item
- Definir se `eta_risk` precisa de histórico append-only (`fact_eta_history`) para sustentar a dimensão "Lead Time" do critério de sucesso — decidir na Fase 5/9, não bloqueia Fase 1.

### Next
- Fase 1: Solution Architecture (Context Diagram, Data Flow, Deployment Architecture).
