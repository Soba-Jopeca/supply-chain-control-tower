# src/transformations

Lógica de transformação Bronze -> Silver -> Gold: casting, normalização, deduplicação, padronização, regras de negócio, integridade referencial.

Fases: 5 (Silver), 7 (Incremental/CDC), 8 (Gold).

Princípio: funções puras e testáveis isoladamente (testadas em `tests/unit`), sem lógica de qualidade misturada (ver `src/quality`).
