# src/ingestion

Código Python reutilizável para leitura das fontes (ERP, TMS, WMS, GPS, Assets, Master Data) e escrita na camada Bronze com metadados de ingestão (`_ingestion_timestamp`, `_source_system`, `_source_file`, `_batch_id`, `_record_hash`, `_ingestion_date`).

Fase do roadmap: Fase 3 (Ingestion/Landing) e Fase 4 (Bronze).

Princípio: idempotência. Reprocessar o mesmo arquivo não pode duplicar registros (usar `_record_hash` + MERGE ou `_batch_id` + delete-insert controlado).
