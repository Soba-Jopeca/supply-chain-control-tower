# Contributing

Este é um projeto de portfólio individual, mas segue fluxo de engenharia como se fosse um time real — isso é parte do que ele demonstra.

## Fluxo

```
feature branch → commit (conventional commits) → push → Pull Request → CI (lint + test) → review → merge
```

## Conventional Commits

```
feat: nova funcionalidade
fix: correção de bug
docs: documentação
refactor: refatoração sem mudança de comportamento
test: testes
chore: manutenção (deps, config)
perf: melhoria de performance (referenciar experimento em docs/performance/)
```

## Antes de abrir PR

- [ ] `pytest` passando (unit + integration + data_quality quando aplicável)
- [ ] `ruff check` e `ruff format` sem pendências
- [ ] `pre-commit run --all-files` limpo
- [ ] Documentação atualizada (README do módulo, ADR se for decisão de arquitetura)
- [ ] Sem hardcode de config/secrets

## Decisões de arquitetura

Nenhuma decisão relevante é feita silenciosamente. Toda decisão de arquitetura vira um ADR em `docs/adr/` usando `docs/adr/ADR-template.md`.
