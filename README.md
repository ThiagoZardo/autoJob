# AutoJob

AutoJob é uma plataforma autônoma de busca e candidatura a vagas cujo objetivo é **maximizar entrevistas qualificadas por unidade de custo**. O produto prioriza oportunidades aderentes, reduz intervenção rotineira e mantém veracidade, conformidade, idempotência e rastreabilidade como restrições fundamentais.

## Arquitetura geral

O desenho de referência é serverless e orientado a eventos na AWS. Descoberta agendada alimenta estágios desacoplados por SQS; Lambdas normalizam, deduplicam, filtram e orquestram avaliações; DynamoDB mantém estado; S3 armazena artefatos versionados; CloudWatch fornece observabilidade. Integrações com fontes e canais de candidatura usam adaptadores. IA só é acionada após filtros determinísticos, e automação de navegador permanece opcional e sob demanda.

## Status atual

**Tarefa 001 — fundação documental concluída.** O repositório contém somente a especificação adaptada, as regras permanentes para agentes e o planejamento incremental. Não há código funcional, dependências instaladas nem infraestrutura provisionada.

## Documentação

- [AGENTS.md](AGENTS.md): regras permanentes de escopo, arquitetura, segurança, qualidade, IA e custos.
- [docs/PRD-SDD.md](docs/PRD-SDD.md): fonte de verdade versionada do produto e do desenho de sistema.
- [docs/ROADMAP.md](docs/ROADMAP.md): épicos, dependências, gates e ordem recomendada.
- [docs/TASKS.md](docs/TASKS.md): backlog de tasks pequenas, com critérios de aceite e rastreabilidade ao backlog original.
- `docs/ADR/` (futuro): decisões arquiteturais que detalhem ou alterem o desenho de referência.
- `docs/runbooks/` (futuro): procedimentos operacionais de execução e recuperação.

## Modelo incremental de desenvolvimento

O trabalho é organizado em tasks preferencialmente XS/S, cada uma executável e verificável em uma sessão focada. Uma task deve ter um único objetivo, dependências explícitas, critérios observáveis, testes e itens fora de escopo. Os épicos avançam por um vertical slice: uma fonte permitida, um pipeline completo e um canal de candidatura suportado antes da expansão para novos adaptadores.

Antes de qualquer implementação, leia `AGENTS.md`, confirme a task atual em `docs/TASKS.md` e consulte `docs/PRD-SDD.md` e `docs/ROADMAP.md`. Não antecipe trabalho futuro.
