# Backlog incremental do AutoJob

## Como usar este arquivo

Este backlog adapta os itens `T001–T037` do PRD/SDD v1.0 para tasks pequenas, preferencialmente executáveis em uma sessão Codex. Os novos IDs são estáveis por épico (`AJ-E##-###`); o campo **Origem** mantém a rastreabilidade ao backlog inicial.

Tamanhos:

- **XS:** alteração isolada, baixa incerteza, verificação direta.
- **S:** uma entrega pequena com testes/documentação, adequada a uma sessão focada.
- **M:** múltiplos componentes acoplados; deve ser novamente avaliada antes de ir para `Ready`.
- **L:** não deve ir para `Ready`; precisa ser dividida.

Prioridades:

- **P0:** bloqueador da fundação/vertical slice ou guardrail obrigatório de segurança, custo e veracidade.
- **P1:** necessário ao MVP, mas dependente do fluxo principal já estabelecido.
- **P2:** expansão, otimização ou componente opcional/pós-MVP.

Tipos:

- **Feature:** comportamento ou contrato do produto.
- **Infra:** toolchain, CI, observabilidade ou infraestrutura reproduzível.
- **Test:** fixture, integração, validação ou exercício operacional cujo resultado principal é evidência.
- **Docs:** documentação, decisão ou governança do projeto.
- **Security:** controle de segurança, privacidade, conformidade ou proteção contra operação insegura.
- **Bug:** correção de comportamento divergente; bugs novos são registrados via template e não fazem parte deste backlog inicial.

Regra: dependências concluídas não autorizam iniciar automaticamente uma task. O usuário deve definir a task atual. Todos os itens futuros estão em backlog, salvo indicação explícita.

## Definition of Done comum

Além dos critérios específicos:

- verificações, lint, typecheck e testes aplicáveis passam;
- nenhum segredo, PII ou dado real do candidato é commitado/logado;
- documentação afetada é atualizada;
- impacto de segurança/custo é registrado;
- trabalho fora do objetivo não é antecipado.

## Epic E0 — Repository foundation

### AJ-E00-001 — Estabelecer fundação documental

- **Epic:** E0 — Repository foundation
- **Objetivo:** versionar a especificação, regras permanentes, roadmap, backlog granular e visão do repositório.
- **Dependências:** nenhuma.
- **Critérios de aceite:** `AGENTS.md`, `README.md`, `docs/PRD-SDD.md`, `docs/ROADMAP.md` e `docs/TASKS.md` existem; os cinco documentos estão consistentes; nenhum código, dependência ou recurso AWS foi criado.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Docs
- **Origem:** solicitação TAREFA 001 + T002 (regras de agentes). **Estado: concluída nesta entrega.**

### AJ-E00-002 — Registrar decisão da toolchain TypeScript

- **Epic:** E0
- **Objetivo:** escolher e documentar gerenciador de pacotes, layout do workspace, versão de Node, test runner e formatter.
- **Dependências:** AJ-E00-001.
- **Critérios de aceite:** ADR compara alternativas e custos de manutenção; decisões e comandos esperados ficam explícitos; nenhuma aplicação funcional é criada.
- **Tamanho:** XS
- **Prioridade:** P0
- **Tipo:** Docs
- **Origem:** preparação de T001/T003.

### AJ-E00-003 — Criar workspace TypeScript mínimo

- **Epic:** E0
- **Objetivo:** criar manifests, configuração TypeScript e scripts vazios/mínimos conforme o ADR da toolchain.
- **Dependências:** AJ-E00-002.
- **Critérios de aceite:** instalação é determinística; `typecheck` executa com sucesso; não existe lógica do AutoJob.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** T001.

### AJ-E00-004 — Criar estrutura mínima de diretórios

- **Epic:** E0
- **Objetivo:** criar somente os diretórios necessários ao desenvolvimento inicial e documentar a responsabilidade de cada área.
- **Dependências:** AJ-E00-003.
- **Critérios de aceite:** estrutura segue `PRD-SDD.md`; não há diretórios/boilerplate de épicos futuros sem uso; README aponta a organização.
- **Tamanho:** XS
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** T001.

### AJ-E00-005 — Configurar lint e formatação

- **Epic:** E0
- **Objetivo:** adicionar configuração mínima de lint/formatter para TypeScript e Markdown conforme ADR.
- **Dependências:** AJ-E00-003.
- **Critérios de aceite:** comandos locais detectam violações e passam no baseline; configurações não conflitam.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** T003.

### AJ-E00-006 — Configurar test runner e cobertura inicial

- **Epic:** E0
- **Objetivo:** habilitar testes unitários com um smoke test não funcional e comando de cobertura.
- **Dependências:** AJ-E00-003.
- **Critérios de aceite:** test runner e cobertura executam localmente; fixture não implementa regra do produto.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Test
- **Origem:** T003.

### AJ-E00-007 — Criar CI de verificação

- **Epic:** E0
- **Objetivo:** executar instalação, lint, typecheck e unit tests em pull requests.
- **Dependências:** AJ-E00-005, AJ-E00-006.
- **Critérios de aceite:** workflow usa versões fixadas/seguras; falha em cada classe de erro; não possui deploy AWS.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** T004.

### AJ-E00-008 — Preparar fluxo GitHub Issues e Project

- **Epic:** E0
- **Objetivo:** padronizar Issues, pull requests, campos, status e regras de movimentação do GitHub Project.
- **Dependências:** AJ-E00-001.
- **Critérios de aceite:** templates `task.md`, `bug.md` e pull request existem; `docs/GITHUB-PROJECT.md` define campos/status/WIP; todas as tasks possuem prioridade e tipo; documentos permanecem consistentes.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Docs
- **Origem:** T005 + solicitação TAREFA 002. **Estado: concluída nesta entrega.**

## Epic E1 — AWS foundation

> Nenhuma task deste épico autoriza provisionamento por si só; execução exige que ela seja a task atual e que conta/região/ambiente estejam explicitamente autorizados.

### AJ-E01-001 — Decidir framework e fronteiras de IaC

- **Epic:** E1 — AWS foundation
- **Objetivo:** escolher CDK ou Terraform e definir stacks/módulos e separação dev/prod.
- **Dependências:** AJ-E00-007.
- **Critérios de aceite:** ADR registra alternativas, estado remoto quando aplicável e estratégia de rollback; custo de baseline é considerado.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Docs
- **Origem:** T006.

### AJ-E01-002 — Inicializar projeto IaC sem recursos

- **Epic:** E1
- **Objetivo:** criar projeto IaC validável, ainda sem provisionar serviços do produto.
- **Dependências:** AJ-E01-001.
- **Critérios de aceite:** synth/validate/plan vazio passa; CI valida IaC; nenhum apply/deploy ocorre.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** T006.

### AJ-E01-003 — Definir convenções de ambiente, tags e nomes

- **Epic:** E1
- **Objetivo:** estabelecer configuração por ambiente, tags obrigatórias e nomes/prefixos sem dados pessoais.
- **Dependências:** AJ-E01-002.
- **Critérios de aceite:** dev/prod não colidem; tags de projeto, ambiente e custo são validadas; valores sensíveis ficam fora do Git.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** T006.

### AJ-E01-004 — Definir OIDC e roles de deploy

- **Epic:** E1
- **Objetivo:** modelar trust policy do GitHub OIDC e permissões mínimas de deployment.
- **Dependências:** AJ-E01-003.
- **Critérios de aceite:** não há access keys long-lived; branch/environment claims restringem acesso; policy passa por revisão de menor privilégio.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Security
- **Origem:** T006, requisito de CI/CD.

### AJ-E01-005 — Modelar tabelas e índices DynamoDB em ADR

- **Epic:** E1
- **Objetivo:** definir entidades, chaves, índices, capacidade, TTL/backup e isolamento antes do provisionamento.
- **Dependências:** AJ-E01-003; contratos iniciais de AJ-E03-001 e AJ-E03-002.
- **Critérios de aceite:** padrões de acesso justificam chaves/índices; custo e retenção são estimados; PII é identificada.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Docs
- **Origem:** preparação de T007.

### AJ-E01-006 — Provisionar DynamoDB via IaC

- **Epic:** E1
- **Objetivo:** declarar as tabelas/índices aprovados com criptografia e proteção adequadas.
- **Dependências:** AJ-E01-005.
- **Critérios de aceite:** synth/plan corresponde ao ADR; criptografia e proteção/backup acordados estão ativos; teste de infraestrutura verifica o modelo.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** T007.

### AJ-E01-007 — Provisionar filas de descoberta e scoring

- **Epic:** E1
- **Objetivo:** declarar filas `jobs.raw`/`jobs.score` e respectivas DLQs com redrive configurado.
- **Dependências:** AJ-E01-003.
- **Critérios de aceite:** visibility timeout/redrive são justificados; criptografia e alarmes básicos estão definidos; testes de IaC passam.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** parte de T008.

### AJ-E01-008 — Provisionar fila de candidaturas e DLQ

- **Epic:** E1
- **Objetivo:** declarar a fila de aplicações separadamente, com parâmetros adequados a submissões idempotentes.
- **Dependências:** AJ-E01-003, AJ-E01-007.
- **Critérios de aceite:** fila não permite retry cego por configuração; DLQ e alarmes são declarados; testes de IaC passam.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** parte de T008.

### AJ-E01-009 — Provisionar bucket versionado de artefatos

- **Epic:** E1
- **Objetivo:** declarar S3 privado para currículos e artefatos, com versionamento, criptografia e bloqueio de acesso público.
- **Dependências:** AJ-E01-003.
- **Critérios de aceite:** bucket é privado e criptografado; lifecycle/retention são explícitos; nenhuma PII de fixture é incluída.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** T009.

### AJ-E01-010 — Declarar schedule desabilitado e contrato de discovery

- **Epic:** E1
- **Objetivo:** modelar EventBridge Scheduler e payload mínimo sem habilitar execução autônoma.
- **Dependências:** AJ-E01-007, AJ-E03-001.
- **Critérios de aceite:** schedule nasce desabilitado; payload possui correlation ID/ambiente; target e permissões são mínimos.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** parte de T010.

### AJ-E01-011 — Criar skeleton de discovery Lambda

- **Epic:** E1
- **Objetivo:** criar handler sem descoberta real, com validação de evento e logging redigido.
- **Dependências:** AJ-E01-010, AJ-E00-006.
- **Critérios de aceite:** handler não chama fontes; testes cobrem evento válido/inválido; pacote pode ser sintetizado sem deploy.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** parte de T010.

### AJ-E01-012 — Definir padrão de observabilidade

- **Epic:** E1
- **Objetivo:** padronizar logs JSON, correlation IDs, níveis e regras de redação para componentes AWS.
- **Dependências:** AJ-E01-011.
- **Critérios de aceite:** schema de log e propagação de correlation ID estão documentados/testados; campos sensíveis são redigidos; nenhum alarme é criado nesta task.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** parte de T011.

### AJ-E01-013 — Configurar alarmes operacionais

- **Epic:** E1
- **Objetivo:** declarar alarmes de erro, throttling e DLQ com thresholds e destinos configuráveis.
- **Dependências:** AJ-E01-007, AJ-E01-008, AJ-E01-012.
- **Critérios de aceite:** cada alarme possui métrica, threshold e destino documentados; testes de IaC passam; alarmes não expõem PII.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** parte de T011.

### AJ-E01-014 — Configurar AWS Budget

- **Epic:** E1
- **Objetivo:** declarar orçamento e alertas de custo configuráveis para o ambiente autorizado.
- **Dependências:** AJ-E01-003.
- **Critérios de aceite:** thresholds e destinatários não são hard-coded; synth/plan mostra somente recursos esperados; nenhum schedule é habilitado.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** parte de T011 e FR-14.

### AJ-E01-015 — Implementar kill switch de schedules

- **Epic:** E1
- **Objetivo:** criar mecanismo reversível que desabilite descoberta/candidatura sem destruir filas ou estado.
- **Dependências:** AJ-E01-010, AJ-E01-013, AJ-E01-014.
- **Critérios de aceite:** ativação/desativação é auditável; teste demonstra preservação de fila/estado; comportamento ao atingir budget está documentado.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Security
- **Origem:** parte de T011 e FR-14.

## Epic E2 — Candidate knowledge base

### AJ-E02-001 — Definir schema de CandidateProfile

- **Epic:** E2 — Candidate knowledge base
- **Objetivo:** modelar roles, skills, idiomas, localização e demais fatos verificáveis, distinguindo ausente de falso.
- **Dependências:** AJ-E00-006.
- **Critérios de aceite:** schema é versionado; fixtures são fictícias; campos desconhecidos não recebem defaults que inventem fatos.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T012.

### AJ-E02-002 — Definir schema de preferências e thresholds

- **Epic:** E2
- **Objetivo:** separar preferências de busca/política dos fatos biográficos do candidato.
- **Dependências:** AJ-E02-001.
- **Critérios de aceite:** `minimum_match`, `auto_apply_match`, work mode e exclusões são validados; relações inválidas entre thresholds falham.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T012.

### AJ-E02-003 — Definir catálogo de fatos e metadados de currículo

- **Epic:** E2
- **Objetivo:** modelar fatos com origem/evidência e referências a resume assets sem armazenar conteúdo pessoal no Git.
- **Dependências:** AJ-E02-001.
- **Critérios de aceite:** cada claim pode apontar para evidência; referências inválidas falham; fixtures não contêm PII real.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** expansão de T012 / Epic 2.

### AJ-E02-004 — Implementar validação e versionamento do perfil

- **Epic:** E2
- **Objetivo:** carregar, validar e atribuir versão determinística ao conjunto perfil/preferências/fatos.
- **Dependências:** AJ-E02-002, AJ-E02-003.
- **Critérios de aceite:** input inválido falha fechado; versão muda com alteração relevante; segredos não entram no objeto/log.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** expansão de T012 / Epic 2.

## Epic E3 — Job domain & persistence

### AJ-E03-001 — Definir schema canônico de Job

- **Epic:** E3 — Job domain & persistence
- **Objetivo:** especificar campos, enums, normalização mínima e versão do contrato `Job`.
- **Dependências:** AJ-E00-006.
- **Critérios de aceite:** campos do PRD/SDD estão representados; payload incompleto/malformado falha com motivo; testes de schema passam.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** T013.

### AJ-E03-002 — Definir schemas de Application e OutcomeEvent

- **Epic:** E3
- **Objetivo:** criar contratos versionados para candidatura, evidência de submissão e eventos de resultado.
- **Dependências:** AJ-E03-001.
- **Critérios de aceite:** estados/tipos seguem a especificação; IDs e timestamps são validados; conteúdo sensível é minimizado.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** preparação de T016 e Epics 7/8.

### AJ-E03-003 — Implementar canonicalização e fingerprint de vaga

- **Epic:** E3
- **Objetivo:** produzir identidade determinística a partir de IDs, URL canônica e conteúdo normalizado.
- **Dependências:** AJ-E03-001.
- **Critérios de aceite:** mesma vaga gera fingerprint estável; mudanças relevantes alteram fingerprint; casos de URL/whitespace têm testes.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T014.

### AJ-E03-004 — Implementar decisão de deduplicação

- **Epic:** E3
- **Objetivo:** classificar duplicatas e registrar razão/evidência sem persistência AWS.
- **Dependências:** AJ-E03-003.
- **Critérios de aceite:** source ID, URL e fingerprint são cobertos; falsos positivos críticos têm fixtures; decisão é auditável.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T014.

### AJ-E03-005 — Implementar JobRepository DynamoDB

- **Epic:** E3
- **Objetivo:** persistir/consultar jobs e decisões de dedupe conforme o ADR de dados.
- **Dependências:** AJ-E01-006, AJ-E03-004.
- **Critérios de aceite:** writes condicionais impedem duplicação; testes usam ambiente controlado/local; erros são tipados e não vazam payload.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** T015.

### AJ-E03-006 — Implementar regras puras da máquina de estados

- **Epic:** E3
- **Objetivo:** validar transições permitidas sem efeitos externos.
- **Dependências:** AJ-E03-002.
- **Critérios de aceite:** todas as transições do PRD/SDD têm testes; transição inválida falha fechado; `BLOCKED` é preservado.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T016.

### AJ-E03-007 — Persistir transições atomicamente

- **Epic:** E3
- **Objetivo:** aplicar optimistic/conditional write e registrar histórico/correlation ID.
- **Dependências:** AJ-E01-006, AJ-E03-005, AJ-E03-006.
- **Critérios de aceite:** concorrência não perde atualização; reexecução idempotente é demonstrada; histórico é auditável.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T016.

### AJ-E03-008 — Definir chave e registro de idempotência de submissão

- **Epic:** E3
- **Objetivo:** derivar identidade candidato+vaga e especificar reset explícito seguro.
- **Dependências:** AJ-E02-004, AJ-E03-003, AJ-E03-007.
- **Critérios de aceite:** chave é estável sem expor PII; reset exige razão/auditoria; testes impedem dupla reserva.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Security
- **Origem:** FR-09 e preparação de T031.

## Epic E4 — First discovery source

### AJ-E04-001 — Definir contrato JobSource

- **Epic:** E4 — First discovery source
- **Objetivo:** criar interface de descoberta/normalização, cursor, erros e metadados de auditoria.
- **Dependências:** AJ-E03-001.
- **Critérios de aceite:** contrato não depende de provedor; suporta paginação/cursor e falhas tipadas; testes de contrato compilam.
- **Tamanho:** XS
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** T017.

### AJ-E04-002 — Selecionar primeira fonte e validar conformidade

- **Epic:** E4
- **Objetivo:** escolher uma fonte com acesso permitido e documentar termos, limites, autenticação e custo.
- **Dependências:** AJ-E04-001.
- **Critérios de aceite:** decisão cita evidência vigente; riscos e rate limits estão registrados; fontes protegidas são descartadas.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Security
- **Origem:** primeira parte de T018.

### AJ-E04-003 — Implementar cliente de descoberta da primeira fonte

- **Epic:** E4
- **Objetivo:** buscar `RawJob` e manter cursor, sem normalizar ou persistir.
- **Dependências:** AJ-E04-002.
- **Critérios de aceite:** paginação, rate limit e backoff têm testes com mocks/fixtures; logs não expõem credenciais; nenhuma candidatura ocorre.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T018.

### AJ-E04-004 — Implementar normalizador da primeira fonte

- **Epic:** E4
- **Objetivo:** converter `RawJob` em `Job` canônico com validação e quarentena.
- **Dependências:** AJ-E04-003, AJ-E03-001.
- **Critérios de aceite:** campos mapeados e ausências estão explícitos; payload malformado vai para erro/quarentena; fixtures passam.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T018.

### AJ-E04-005 — Integrar fonte, dedupe e persistência com fixtures

- **Epic:** E4
- **Objetivo:** provar discovery → normalize → dedupe → persist em ambiente de teste.
- **Dependências:** AJ-E04-004, AJ-E03-005.
- **Critérios de aceite:** reexecução não duplica; cursor retoma corretamente; falha transitória e payload malformado são testados.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Test
- **Origem:** T019 e fechamento de T018.

## Epic E5 — Filtering & matching

### AJ-E05-001 — Definir regras de elegibilidade determinística

- **Epic:** E5 — Filtering & matching
- **Objetivo:** especificar hard blocks e razões para keywords, localização, work mode, salário e requisitos obrigatórios.
- **Dependências:** AJ-E02-002, AJ-E03-001.
- **Critérios de aceite:** precedência e campos ausentes são definidos; cada regra tem fixtures de limite; nenhuma LLM é usada.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** preparação de T020.

### AJ-E05-002 — Implementar filtros determinísticos

- **Epic:** E5
- **Objetivo:** produzir decisão auditável de pré-filtro antes do scoring.
- **Dependências:** AJ-E05-001.
- **Critérios de aceite:** reasons são estruturados; hard blocks prevalecem; testes cobrem meta de mensuração de vagas filtradas sem LLM.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** T020.

### AJ-E05-003 — Definir schema MatchEvaluation

- **Epic:** E5
- **Objetivo:** versionar score, strengths, gaps, hard blocks, recomendação e metadados de modelo/prompt.
- **Dependências:** AJ-E03-001.
- **Critérios de aceite:** score/faixas/enums são validados; payload desconhecido falha; schema não permite texto livre substituir hard blocks.
- **Tamanho:** XS
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** T021.

### AJ-E05-004 — Criar prompt versionado e parser estruturado

- **Epic:** E5
- **Objetivo:** definir input mínimo, output JSON e validação sem integrar provedor real.
- **Dependências:** AJ-E02-004, AJ-E05-003.
- **Critérios de aceite:** prompt proíbe facts não verificados; parser rejeita schema inválido; versão é persistível e testada.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T022.

### AJ-E05-005 — Implementar matcher LLM de primeiro passe

- **Epic:** E5
- **Objetivo:** chamar modelo de baixo custo por interface, validar resposta e aplicar uma tentativa de reparo.
- **Dependências:** AJ-E05-002, AJ-E05-004; decisão de provedor/orçamento.
- **Critérios de aceite:** somente elegíveis chegam ao modelo; falha após reparo resulta em erro encaminhável à DLQ; testes não exigem gasto real.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T022.

### AJ-E05-006 — Implementar policy engine de matching

- **Epic:** E5
- **Objetivo:** decidir `IGNORE`, `QUEUE`, `APPLY` ou `BLOCKED` por thresholds, score e hard constraints.
- **Dependências:** AJ-E02-002, AJ-E05-003, AJ-E05-005.
- **Critérios de aceite:** decisão é determinística e explicável; hard block sempre produz `BLOCKED`; limites têm testes.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** T023.

### AJ-E05-007 — Adicionar cache de avaliações

- **Epic:** E5
- **Objetivo:** evitar reavaliação por fingerprint da vaga e versão do perfil/prompt.
- **Dependências:** AJ-E03-003, AJ-E05-005.
- **Critérios de aceite:** cache invalida com mudança relevante; hit não chama modelo; chave não contém PII em claro.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte da estratégia de T022.

### AJ-E05-008 — Adicionar telemetria de token e custo

- **Epic:** E5
- **Objetivo:** registrar uso de tokens, modelo e custo estimado por avaliação sem persistir prompt ou PII.
- **Dependências:** AJ-E05-005.
- **Critérios de aceite:** métricas distinguem chamada real de cache hit; cálculo e ausência de preço têm testes; logs não contêm conteúdo sensível.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Infra
- **Origem:** T024.

## Epic E6 — Application package generator

### AJ-E06-001 — Definir schema ApplicationPackage

- **Epic:** E6 — Application package generator
- **Objetivo:** versionar currículo/variante, respostas, evidências de fatos e campos `UNSUPPORTED`.
- **Dependências:** AJ-E02-003, AJ-E03-002.
- **Critérios de aceite:** toda claim referencia fato/evidência; ausência factual é representável; schema impede pacote incompleto pronto para envio.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** T025.

### AJ-E06-002 — Implementar extrator de claims do pacote

- **Epic:** E6
- **Objetivo:** decompor conteúdo gerado em claims verificáveis antes da validação.
- **Dependências:** AJ-E06-001.
- **Critérios de aceite:** fixtures cobrem datas, skills, senioridade e números; falha de extração bloqueia readiness; sem LLM real em testes.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T026.

### AJ-E06-003 — Implementar validador de veracidade

- **Epic:** E6
- **Objetivo:** comparar claims ao catálogo de fatos e produzir supported/unsupported/conflict.
- **Dependências:** AJ-E02-003, AJ-E06-002.
- **Critérios de aceite:** nenhuma claim sem evidência passa; conflito bloqueia pacote; testes incluem zero tolerância a invenção.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Security
- **Origem:** parte de T026.

### AJ-E06-004 — Criar prompt versionado de respostas

- **Epic:** E6
- **Objetivo:** solicitar respostas usando somente fatos selecionados e exigir `UNSUPPORTED` quando necessário.
- **Dependências:** AJ-E06-001, AJ-E05-004.
- **Critérios de aceite:** input contém apenas dados mínimos; output é estruturado; casos sem fato não induzem adivinhação.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T027.

### AJ-E06-005 — Implementar gerador de respostas personalizadas

- **Epic:** E6
- **Objetivo:** gerar respostas, validar schema e passar pelo guardrail de veracidade.
- **Dependências:** AJ-E06-003, AJ-E06-004.
- **Critérios de aceite:** pacote com claim inválida não fica `READY`; modelo é mockável; custo e versões são registrados.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T027.

### AJ-E06-006 — Implementar conteúdo de currículo por variante

- **Epic:** E6
- **Objetivo:** selecionar/reordenar fatos verificados para uma vaga sem alterar verdade factual.
- **Dependências:** AJ-E06-003, AJ-E05-006.
- **Critérios de aceite:** conteúdo é rastreável aos fatos; não cria experiência/skills; variantes possuem ID/versionamento.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T027.

### AJ-E06-007 — Persistir artefatos gerados no S3

- **Epic:** E6
- **Objetivo:** armazenar pacote e artefatos versionados com criptografia, metadados mínimos e referência no estado.
- **Dependências:** AJ-E01-009, AJ-E06-005, AJ-E06-006.
- **Critérios de aceite:** objetos não são públicos; persistência é idempotente; logs não contêm conteúdo do currículo/respostas.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** T028.

## Epic E7 — First submission adapter

### AJ-E07-001 — Definir contrato ApplicationAdapter

- **Epic:** E7 — First submission adapter
- **Objetivo:** especificar `supports`, `prepare`, `submit`, evidência, erros e reconciliação.
- **Dependências:** AJ-E03-002, AJ-E06-001.
- **Critérios de aceite:** contrato é independente de canal; distingue falha, bloqueio e resultado incerto; idempotency key é obrigatória.
- **Tamanho:** XS
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** T029.

### AJ-E07-002 — Selecionar primeiro canal e validar conformidade

- **Epic:** E7
- **Objetivo:** escolher um canal suportado com ambiente/teste permitido, termos, limites e custo documentados.
- **Dependências:** AJ-E07-001, AJ-E04-002.
- **Critérios de aceite:** acesso programático é permitido; fluxo protegido é excluído; estratégia segura de teste está definida.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Security
- **Origem:** primeira parte de T030.

### AJ-E07-003 — Implementar supports e prepare do primeiro adaptador

- **Epic:** E7
- **Objetivo:** decidir compatibilidade e mapear `ApplicationPackage` para request sem enviar.
- **Dependências:** AJ-E07-002, AJ-E06-007.
- **Critérios de aceite:** incompatibilidades geram `BLOCKED`; request é validado e testado com fixtures; nenhum envio real ocorre.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T030.

### AJ-E07-004 — Implementar submit e captura de evidência

- **Epic:** E7
- **Objetivo:** executar uma submissão controlada por interface e persistir referência/evidência sanitizada.
- **Dependências:** AJ-E07-003, AJ-E03-008.
- **Critérios de aceite:** testes usam sandbox/mock autorizado; sucesso possui referência auditável; PII não aparece em logs.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T030.

### AJ-E07-005 — Aplicar idempotência na submissão

- **Epic:** E7
- **Objetivo:** reservar/confirmar a chave idempotente em torno do envio.
- **Dependências:** AJ-E07-004, AJ-E03-008.
- **Critérios de aceite:** reexecução não envia duas vezes; concorrência tem teste; reset explícito permanece auditado.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Security
- **Origem:** parte de T031.

### AJ-E07-006 — Implementar reconciliação, retry e DLQ

- **Epic:** E7
- **Objetivo:** tratar falhas transitórias e resultado incerto sem retry cego.
- **Dependências:** AJ-E01-008, AJ-E07-005.
- **Critérios de aceite:** resultado incerto reconcilia antes de retry; backoff/DLQ são testados; CAPTCHA/MFA vira `BLOCKED`.
- **Tamanho:** S
- **Prioridade:** P0
- **Tipo:** Feature
- **Origem:** parte de T031 e fechamento de T030.

## Epic E8 — Inbox & interview detection

### AJ-E08-001 — Decidir mailbox, escopo e autorização

- **Epic:** E8 — Inbox & interview detection
- **Objetivo:** escolher mailbox/label dedicada, provedor e acesso mínimo.
- **Dependências:** AJ-E07-006.
- **Critérios de aceite:** ADR exclui leitura indiscriminada; credenciais/retention são definidos; custos e revogação são documentados.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Security
- **Origem:** preparação de T032.

### AJ-E08-002 — Implementar ingestão e normalização de mensagens

- **Epic:** E8
- **Objetivo:** obter metadados/conteúdo mínimo, deduplicar por message ID e emitir evento sanitizado.
- **Dependências:** AJ-E08-001, AJ-E03-002.
- **Critérios de aceite:** reprocessamento é idempotente; logs redigem conteúdo/endereços; fixtures cobrem payloads inválidos.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Feature
- **Origem:** T032.

### AJ-E08-003 — Implementar classificador de resposta

- **Epic:** E8
- **Objetivo:** classificar recruiter reply, interview, rejection, assessment, confirmation e unrelated com confiança.
- **Dependências:** AJ-E08-002, AJ-E05-004.
- **Critérios de aceite:** schema/threshold são validados; baixa confiança exige revisão/não muda estado crítico; fixtures passam sem e-mail real.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Feature
- **Origem:** T033.

### AJ-E08-004 — Implementar correlação mensagem–candidatura

- **Epic:** E8
- **Objetivo:** correlacionar por referência, domínio/empresa, cargo e thread, com score/evidência.
- **Dependências:** AJ-E08-003, AJ-E03-007.
- **Critérios de aceite:** correlação ambígua não escolhe arbitrariamente; decisão é auditável; fixtures cobrem colisões.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Feature
- **Origem:** T034.

### AJ-E08-005 — Implementar transição e alerta de entrevista

- **Epic:** E8
- **Objetivo:** atualizar estado em alta confiança e notificar o usuário sem aceitar agenda/termos.
- **Dependências:** AJ-E08-004, AJ-E01-012.
- **Critérios de aceite:** somente evento correlacionado e acima do threshold atualiza estado; alerta é conciso e redigido; não há ação automática de calendário.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Feature
- **Origem:** T035.

## Epic E9 — Metrics & optimization

### AJ-E09-001 — Definir eventos e dimensões de métricas

- **Epic:** E9 — Metrics & optimization
- **Objetivo:** especificar métricas por role, fonte, empresa, score band, variante e custos, sem cardinalidade/PII indevidas.
- **Dependências:** AJ-E05-008, AJ-E07-006, AJ-E08-005.
- **Critérios de aceite:** numeradores/denominadores estão definidos; entrevista é medida desde o início; política de retenção está explícita.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Feature
- **Origem:** preparação de T036.

### AJ-E09-002 — Implementar agregação de conversão

- **Epic:** E9
- **Objetivo:** agregar vagas, candidaturas, respostas e entrevistas pelas dimensões aprovadas.
- **Dependências:** AJ-E09-001.
- **Critérios de aceite:** reprocessamento não duplica contagem; fixtures cobrem janelas e eventos tardios; resultados são reproduzíveis.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Feature
- **Origem:** parte de T036.

### AJ-E09-003 — Implementar agregação de custos

- **Epic:** E9
- **Objetivo:** calcular custo estimado por vaga, candidatura e entrevista.
- **Dependências:** AJ-E05-008, AJ-E09-001.
- **Critérios de aceite:** unidades/moeda e ausência de preço são tratadas; cache não duplica custo; cálculo possui testes.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Feature
- **Origem:** parte de T036.

### AJ-E09-004 — Criar dashboard operacional mínimo

- **Epic:** E9
- **Objetivo:** expor saúde de filas/DLQs, erros, orçamento, volume, conversão e custos.
- **Dependências:** AJ-E01-012, AJ-E09-002, AJ-E09-003.
- **Critérios de aceite:** dashboard não exibe PII; alarmes essenciais têm links/contexto; IaC valida sem mudança destrutiva.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Infra
- **Origem:** parte de T037.

### AJ-E09-005 — Criar runbook de controle operacional

- **Epic:** E9
- **Objetivo:** documentar pause/resume, uso do kill switch, verificação de estado e rollback operacional.
- **Dependências:** AJ-E01-015, AJ-E09-004.
- **Critérios de aceite:** passos têm pré-condições e verificação; rollback preserva filas/estado; procedimento não exige credenciais no documento.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Docs
- **Origem:** parte de T037 e critérios de release.

### AJ-E09-006 — Criar runbook de recuperação e segurança

- **Epic:** E9
- **Objetivo:** documentar recuperação de DLQ, reconciliação de submissão e rotação/revogação de segredos.
- **Dependências:** AJ-E07-006, AJ-E09-004.
- **Critérios de aceite:** cada procedimento possui condição de entrada/saída e evidência; não há segredos no documento; casos `BLOCKED` não recebem bypass.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Docs
- **Origem:** parte de T037 e critérios de release.

### AJ-E09-007 — Preparar teste operacional de sete dias

- **Epic:** E9
- **Objetivo:** definir janela, ambiente, métricas, critérios de interrupção e coleta de evidência para o soak test do MVP.
- **Dependências:** AJ-E09-005, AJ-E09-006; gates do Milestone 1 concluídos.
- **Critérios de aceite:** checklist de início/fim e rollback está documentado; budget e kill switch são pré-condições; nenhuma execução é iniciada nesta task.
- **Tamanho:** S
- **Prioridade:** P1
- **Tipo:** Test
- **Origem:** critério de release e milestone recomendado.

### AJ-E09-008 — Executar e avaliar teste operacional de sete dias

- **Epic:** E9
- **Objetivo:** executar o plano aprovado e avaliar operação sem intervenção rotineira, exceto eventos externos previstos.
- **Dependências:** AJ-E09-007.
- **Critérios de aceite:** período, incidentes, custos e intervenções são registrados; critérios do MVP são avaliados; falhas viram novas tasks, sem correções fora de escopo.
- **Tamanho:** S (duração de calendário não implica trabalho contínuo)
- **Prioridade:** P1
- **Tipo:** Test
- **Origem:** critério de release e milestone recomendado.

## Epic E10 — Additional sources/adapters

### AJ-E10-001 — Priorizar próximo adaptador com evidência

- **Epic:** E10 — Additional sources/adapters
- **Objetivo:** selecionar uma única nova fonte ou canal por valor, cobertura, conversão, conformidade e custo observados.
- **Dependências:** AJ-E09-008.
- **Critérios de aceite:** decisão usa métricas de produção; termos/acesso são verificados; browser não é assumido.
- **Tamanho:** S
- **Prioridade:** P2
- **Tipo:** Docs
- **Origem:** Epic 10 e pós-MVP.

### AJ-E10-002 — Implementar próximo adaptador pelo contrato existente

- **Epic:** E10
- **Objetivo:** adicionar um adaptador sem alterar matching/domínio salvo ADR indispensável.
- **Dependências:** AJ-E10-001; contrato JobSource ou ApplicationAdapter correspondente.
- **Critérios de aceite:** adapter suporta paginação/idempotência/erros aplicáveis; não duplica arquitetura; fixtures sanitizadas passam.
- **Tamanho:** S
- **Prioridade:** P2
- **Tipo:** Feature
- **Origem:** Epic 10.

### AJ-E10-003 — Validar novo adaptador em integração controlada

- **Epic:** E10
- **Objetivo:** provar operação, observabilidade, custo e rollback antes de habilitação gradual.
- **Dependências:** AJ-E10-002.
- **Critérios de aceite:** evidência cobre sucesso/falhas/limites; kill switch e métricas funcionam; habilitação ampla permanece fora de escopo.
- **Tamanho:** S
- **Prioridade:** P2
- **Tipo:** Test
- **Origem:** Epic 10.

## Epic E11 — Browser worker (opcional)

### AJ-E11-001 — Justificar browser worker em ADR

- **Epic:** E11 — Browser worker
- **Objetivo:** demonstrar que um fluxo permitido de alto valor não pode usar API/ATS e definir custo/risco.
- **Dependências:** AJ-E09-008, AJ-E10-001.
- **Critérios de aceite:** alternativas são rejeitadas com evidência; termos permitem automação; CAPTCHA/MFA/bypass permanecem proibidos.
- **Tamanho:** S
- **Prioridade:** P2
- **Tipo:** Docs
- **Origem:** Epic 11 / ADR-005.

### AJ-E11-002 — Criar runtime efêmero do browser worker

- **Epic:** E11
- **Objetivo:** criar runtime sob demanda com timeout e encerramento garantido, ainda sem integrar um fluxo real.
- **Dependências:** AJ-E11-001, AJ-E01-015.
- **Critérios de aceite:** worker nasce desligado; termina após job/timeout; limite de custo/recursos é configurável; não acessa sites nesta task.
- **Tamanho:** S
- **Prioridade:** P2
- **Tipo:** Infra
- **Origem:** Epic 11.

### AJ-E11-003 — Aplicar isolamento e controles de segurança ao worker

- **Epic:** E11
- **Objetivo:** aplicar menor privilégio, isolamento de rede/segredos e redação de logs/artefatos ao runtime.
- **Dependências:** AJ-E11-002.
- **Critérios de aceite:** permissões são mínimas; secrets não aparecem em logs/imagens; testes validam encerramento e negação de acesso indevido.
- **Tamanho:** S
- **Prioridade:** P2
- **Tipo:** Security
- **Origem:** Epic 11.

### AJ-E11-004 — Integrar um fluxo browser suportado

- **Epic:** E11
- **Objetivo:** conectar um único adaptador compatível ao worker com idempotência e detecção de bloqueio.
- **Dependências:** AJ-E11-003, AJ-E07-006.
- **Critérios de aceite:** CAPTCHA/MFA/proteção gera `BLOCKED`; resultado incerto reconcilia antes de retry; testes não burlam controles.
- **Tamanho:** S
- **Prioridade:** P2
- **Tipo:** Feature
- **Origem:** Epic 11.

## Mapa de rastreabilidade do backlog original

| Original | Tasks adaptadas |
| --- | --- |
| T001 | AJ-E00-002 a AJ-E00-004 |
| T002 | AJ-E00-001 |
| T003 | AJ-E00-005, AJ-E00-006 |
| T004 | AJ-E00-007 |
| T005 | AJ-E00-008 |
| T006 | AJ-E01-001 a AJ-E01-004 |
| T007 | AJ-E01-005, AJ-E01-006 |
| T008 | AJ-E01-007, AJ-E01-008 |
| T009 | AJ-E01-009 |
| T010 | AJ-E01-010, AJ-E01-011 |
| T011 | AJ-E01-012 a AJ-E01-015 |
| T012 | AJ-E02-001 a AJ-E02-004 |
| T013 | AJ-E03-001 |
| T014 | AJ-E03-003, AJ-E03-004 |
| T015 | AJ-E03-005 |
| T016 | AJ-E03-006, AJ-E03-007 |
| T017 | AJ-E04-001 |
| T018 | AJ-E04-002 a AJ-E04-004 |
| T019 | AJ-E04-005 |
| T020 | AJ-E05-001, AJ-E05-002 |
| T021 | AJ-E05-003 |
| T022 | AJ-E05-004, AJ-E05-005, AJ-E05-007 |
| T023 | AJ-E05-006 |
| T024 | AJ-E05-008 |
| T025 | AJ-E06-001 |
| T026 | AJ-E06-002, AJ-E06-003 |
| T027 | AJ-E06-004 a AJ-E06-006 |
| T028 | AJ-E06-007 |
| T029 | AJ-E07-001 |
| T030 | AJ-E07-002 a AJ-E07-004, AJ-E07-006 |
| T031 | AJ-E07-005, AJ-E07-006 |
| T032 | AJ-E08-001, AJ-E08-002 |
| T033 | AJ-E08-003 |
| T034 | AJ-E08-004 |
| T035 | AJ-E08-005 |
| T036 | AJ-E09-001 a AJ-E09-003 |
| T037 | AJ-E09-004 a AJ-E09-006 |

Os itens E10/E11 e AJ-E09-006 detalham épicos/critérios que existiam no PRD/SDD sem IDs próprios no backlog inicial. Nenhuma task `M` ou `L` permanece: as antigas `M/L` foram divididas em entregas `XS/S` com gates explícitos.
