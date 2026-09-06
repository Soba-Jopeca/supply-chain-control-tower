# pipelines/bronze

Notebooks finos de orquestração da camada Bronze — chamam funções de `src/ingestion/`, não implementam lógica de negócio aqui.

Princípio (ADR-0001): Bronze fica o mais próxima possível do dado original. Sem correção de semântica de negócio nesta camada.
