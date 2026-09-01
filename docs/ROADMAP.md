# Roadmap do AutoJob

## Propósito

Este roadmap organiza os épicos definidos no PRD/SDD v1.0, explicita dependências e recomenda uma ordem de implementação. Ele não autoriza iniciar um épico: cada mudança deve estar vinculada a uma task pequena em `TASKS.md`.

## Princípios de sequenciamento

- Provar um vertical slice completo antes de expandir fontes ou canais.
- Estabelecer contratos, segurança, observabilidade e limites de custo antes de execução autônoma.
- Manter filtro determinístico antes de IA.
- Implementar browser worker somente após evidência de que um fluxo permitido não pode usar API/ATS.
- Tratar veracidade, idempotência, auditabilidade e kill switch como gates, não melhorias posteriores.

## Épicos e dependências

| Epic | Resultado esperado | Dependências mínimas | Gate de saída |
| --- | --- | --- | --- |
| E0 — Repository foundation | Repositório, regras, toolchain, testes, CI e templates prontos para entregas pequenas. | Nenhuma | Verificações locais e CI reproduzíveis; documentação consistente. |
| E1 — AWS foundation | IaC para ambientes, IAM/OIDC, DynamoDB, SQS/DLQ, S3, EventBridge, logs, alarmes, budgets e kill switch. | E0; decisão de IaC | Infra validada por synth/plan; nenhum schedule autônomo ativo sem guardrails. |
| E2 — Candidate knowledge base | Schemas versionados de perfil, preferências, fatos e currículos, com validação e carregamento seguro. | E0 | Dados inválidos/fatos ausentes são rejeitados ou marcados `UNSUPPORTED`; sem PII no Git. |
| E3 — Job domain & persistence | Modelos canônicos, fingerprint, dedupe, repositórios, estados e idempotência. | E0; E1 para persistência implantada | Duplicidade e transições cobertas por testes; escrita idempotente demonstrada. |
| E4 — First discovery source | Uma fonte permitida operando descoberta e normalização ponta a ponta. | E0, E2, E3; E1 para execução AWS | Fixtures e testes de integração; cursor, erros e auditoria definidos. |
| E5 — Filtering & matching | Filtros determinísticos, avaliação estruturada, política, cache, prompts versionados e telemetria de custo. | E2, E3, E4 | Hard blocks precedem LLM; schema/política testados; custo mensurável. |
| E6 — Application package generator | Pacote de candidatura, respostas/currículo personalizados, veracidade e artefatos versionados. | E2, E3, E5; E1 para S3 | Nenhuma claim não verificada; casos desconhecidos resultam em `UNSUPPORTED`. |
| E7 — First submission adapter | Um canal permitido prepara, submete, registra evidência e reconcilia incertezas. | E1, E3, E6 | Testes demonstram nenhuma submissão duplicada e retentativa segura. |
| E8 — Inbox & interview detection | Mensagens de caixa dedicada são classificadas, correlacionadas e geram alertas. | E1, E3, E7 | Fixtures cobrem classificação/correlação; sem aceitação automática de termos ou agenda. |
| E9 — Metrics & optimization | Métricas de conversão/custo, scorecards, dashboards, runbooks e avaliação operacional. | E1, E3, E5, E7, E8 | Custos e conversão medidos; runbook e teste de operação contínua concluídos. |
| E10 — Additional sources/adapters | Novos adaptadores adicionados individualmente com base em evidência de produção. | E4, E7, E9 | Cada adaptador passa pelo mesmo contrato, compliance review e fixtures. |
| E11 — Browser worker | Worker isolado, efêmero e sob demanda para um fluxo permitido que realmente exija navegador. | E7, E9; ADR de justificativa | Custo, isolamento, kill switch e encerramento imediato comprovados. |

## Ordem recomendada

```text
E0
├── E1 ───────────────┬───────────────┐
├── E2 ──┐            │               │
└── E3 <─┴─ E1*       │               │
         │            │               │
         └── E4 ── E5 ┴── E6 ── E7 ── E8 ── E9
                                      │      │
                                      └──────┴── E10
                                             └── E11 (opcional, via ADR)

* Partes de domínio de E3 podem avançar antes da implantação de E1;
  persistência integrada depende da fundação AWS.
```

### Fases sugeridas

1. **Fundação de entrega:** concluir E0 e registrar as decisões ainda abertas (toolchain e IaC).
2. **Contratos e plataforma:** avançar E1, E2 e a parte de domínio de E3; integrar persistência depois da base IaC.
3. **Primeiro fluxo de entrada:** concluir E4 com uma única fonte permitida.
4. **Decisão e preparação:** concluir E5 e E6 com veracidade e custo observáveis.
5. **Primeiro fluxo de saída:** concluir E7 com idempotência e reconciliação.
6. **Feedback e operação:** concluir E8 e E9; executar o gate operacional do MVP.
7. **Expansão baseada em evidência:** adicionar E10 um adaptador por vez; considerar E11 somente se justificado.

## Milestone 1 — Vertical slice do MVP

Fluxo-alvo:

```text
discover → normalize → dedupe → filter → score → prepare
→ submit → persist → detect response
```

Condição de saída: um fluxo autônomo em ambiente AWS de desenvolvimento, com uma fonte e um canal suportados, testes, trilha de auditoria, telemetria de custo, alertas e kill switch. O fluxo deve operar sem rotina manual por sete dias, exceto eventos externos de autenticação/proteção.

## Gates transversais

| Gate | Deve existir antes de |
| --- | --- |
| Veracidade e `UNSUPPORTED` | Qualquer conteúdo de candidatura ser considerado pronto. |
| Idempotência e reconciliação | Qualquer submissão real ou retentativa. |
| Budgets, alarmes e kill switch | Habilitar schedules ou processamento autônomo pago. |
| Compliance review da fonte/canal | Integrar ou automatizar uma fonte/canal. |
| Redação de logs e minimização de dados | Processar PII, e-mail ou currículo. |
| Fixtures e testes de integração | Declarar um adaptador suportado. |
| Runbooks de pausa/recuperação | Operação contínua ou promoção de ambiente. |

## Fora do roadmap do MVP

Ficam pós-MVP: SaaS público multi-tenant, ranking adaptativo por role/source, A/B de currículo, reputação de empresas, normalização salarial multimoeda, outreach separado, dashboard de produto completo e thresholds adaptativos. Também permanecem proibidos bypass de proteções, candidatura indiscriminada, personificação, presença automática em entrevistas e negociação/aceitação automática de ofertas.
