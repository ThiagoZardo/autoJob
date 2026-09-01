# AGENTS.md — AutoJob

Este arquivo contém regras permanentes para qualquer agente que trabalhe neste repositório. Ele se aplica a todo o projeto, salvo instrução mais específica em um `AGENTS.md` descendente. A solicitação explícita do usuário para a task atual sempre prevalece.

## 1. Objetivo do produto

O AutoJob é uma plataforma autônoma de busca e candidatura a vagas. Seu objetivo norteador é **maximizar entrevistas qualificadas por unidade de custo**, com mínima intervenção rotineira, sem sacrificar veracidade, segurança, conformidade ou rastreabilidade.

O MVP deve descobrir vagas em fontes permitidas, normalizá-las, eliminar duplicatas, aplicar filtros determinísticos, avaliar aderência, preparar materiais verdadeiros, submeter candidaturas somente por canais suportados, acompanhar estados e identificar respostas relevantes.

## 2. Fonte de verdade e hierarquia documental

Antes de decisões de produto ou arquitetura, consulte:

1. `docs/PRD-SDD.md`: requisitos e desenho de referência;
2. `docs/ROADMAP.md`: dependências e ordem dos épicos;
3. `docs/TASKS.md`: unidades de trabalho e critérios de aceite;
4. `docs/ADR/`, quando existir: decisões arquiteturais aprovadas;
5. a descrição da task atual.

Não contradiga essa documentação silenciosamente. Se uma decisão precisar mudar, registre a justificativa em ADR e atualize os documentos afetados na mesma task, desde que isso esteja no escopo autorizado.

## 3. Disciplina de escopo

- Execute somente o objetivo e os critérios de aceite da task atual.
- Não antecipe épicos ou tasks futuras por conveniência.
- Não faça refatorações, provisões, integrações ou automações adjacentes sem necessidade demonstrável para a task atual.
- Se um requisito essencial estiver ambíguo e alterar materialmente o resultado, pare e peça decisão; não invente escopo.
- Preserve mudanças existentes do usuário e mantenha cada entrega pequena, reversível e verificável.
- Ao concluir, pare. Não inicie automaticamente a próxima task.

## 4. Princípios arquiteturais

- AWS serverless e orientada a eventos por padrão, visando baixo custo ocioso.
- Filas entre estágios para desacoplamento, backpressure, retentativas e escalabilidade independente.
- Processamento assíncrono; não há requisito de latência subsegundo no MVP.
- Consumidores idempotentes e compatíveis com entrega pelo menos uma vez.
- Regras de negócio independentes de provedores ATS e de detalhes de infraestrutura.
- Fontes e canais de candidatura implementam interfaces comuns; novos adaptadores não alteram o núcleo.
- Filtro determinístico antes de qualquer chamada a LLM.
- DynamoDB como armazenamento inicial de estado/eventos e S3 para artefatos versionados.
- Automação de navegador é opcional, sob demanda e somente para fluxos permitidos e estáveis.
- Infraestrutura reproduzível por IaC; aplicação e infraestrutura implantáveis separadamente quando prático.
- Logs estruturados, IDs de correlação, trilha de auditoria e estados persistidos em todos os fluxos relevantes.

## 5. Segurança, privacidade e conformidade

- Aplique menor privilégio em IAM e prefira OIDC a credenciais AWS de longa duração.
- Nunca grave segredos, tokens, cookies, credenciais, e-mails, currículos ou PII no Git ou em logs.
- Use armazenamento seguro para segredos e criptografia em repouso e em trânsito.
- Minimize a coleta e a retenção de dados ao necessário para matching, candidatura e análise de resultados.
- Separe ambientes e configurações; produção exige aprovação manual enquanto a documentação não determinar o contrário.
- Não contorne CAPTCHA, MFA, rate limits, anti-bot, termos de uso, controles de acesso ou restrições técnicas.
- Fluxos protegidos, não suportados ou de resultado incerto devem ser marcados `BLOCKED` ou reconciliados; nunca faça retentativa cega de uma possível submissão bem-sucedida.
- Não aceite entrevistas, horários, ofertas ou termos de emprego em nome do candidato.

## 6. Regras para uso de IA

- Use código determinístico para exclusões, localização, salário mínimo, duplicidade e demais hard constraints.
- Use saídas estruturadas e validadas por schema.
- Versione prompts e registre modelo/versão em cada avaliação.
- Use o modelo de menor custo que atenda ao caso; escale somente vagas promissoras ou ambíguas.
- Faça cache por fingerprint da vaga e versão do perfil quando seguro.
- Não envie segredos ou PII desnecessária a modelos.
- Trate saída de IA como não confiável até validação de schema, política e veracidade.

### Restrição absoluta de veracidade

É proibido inventar, completar por inferência ou embelezar fatos do candidato. Isso inclui experiência, habilidades, senioridade, idiomas, localização, disponibilidade, histórico salarial, formação, certificações, credenciais e resultados profissionais.

Conteúdo gerado pode apenas selecionar, reorganizar e reformular fatos verificados na base de conhecimento do candidato. Informação factual ausente deve ser marcada `UNSUPPORTED` e encaminhada para decisão humana; nunca deve ser adivinhada.

## 7. Política de custos

- Filtre antes de chamar LLM e evite reprocessamento com fingerprints, cache e idempotência.
- Prefira Lambda para computação curta e mantenha workers de navegador desligados por padrão.
- Toda feature que consuma AWS, API paga ou LLM deve expor impacto mensurável e limite configurável.
- Registre custo estimado por vaga, candidatura e entrevista quando o fluxo correspondente existir.
- Configure budgets, alarmes e kill switch antes de habilitar execução autônoma agendada.
- Ao atingir limite de orçamento, interrompa processamento opcional sem destruir filas ou estado.
- Não provisione recursos pagos sem autorização explícita da task.

## 8. Qualidade e estratégia de testes

- Escreva código simples, tipado, coeso e orientado a interfaces nos limites do domínio.
- Cada mudança comportamental exige teste proporcional ao risco e documentação atualizada.
- Testes unitários cobrem regras determinísticas, schemas, normalização, deduplicação, políticas, transições de estado e guardrails de veracidade.
- Testes de integração usam fixtures sanitizadas por adaptador e cobrem idempotência, retentativas, DLQ e correlação.
- Integrações externas devem ser testáveis sem operações reais por padrão; submissões reais exigem autorização e ambiente controlado.
- Testes devem incluir caminhos felizes, limites, payloads malformados, falhas transitórias e negação segura.
- O Definition of Done inclui critérios de aceite atendidos, testes/lint/typecheck aprovados, ausência de segredos/PII e evidência de verificação.

## 9. Convenções de desenvolvimento

- TypeScript é a linguagem de referência para aplicação; não escolha gerenciador de pacotes, framework de IaC ou test runner sem task/ADR correspondente.
- Use nomes em inglês para código, schemas, eventos e recursos; documentação pode ser em português.
- Mantenha domínio em `src/domain`, adaptadores nas áreas `src/sources`, `src/applications` e `src/inbox`, e utilitários transversais em `src/shared` quando essas estruturas forem criadas.
- Não hard-code dados pessoais nem configuração de ambiente.
- Valide entradas nas fronteiras e preserve contratos versionados.
- Use commits/PRs pequenos; registre comandos de verificação e implicações de rollback para infraestrutura.
- Não instale dependências sem necessidade explícita; justifique novas dependências e prefira bibliotecas mantidas e de escopo mínimo.

## 10. Checklist antes de encerrar uma task

- O escopo atual foi respeitado e trabalho futuro não foi antecipado.
- A documentação e ADRs relevantes foram consultados.
- Critérios de aceite foram verificados com evidência reproduzível.
- Testes adequados passaram, ou limitações foram declaradas.
- Nenhum segredo, PII ou fato inventado do candidato foi introduzido.
- Impactos de segurança, custo e operação foram avaliados.
- Documentos afetados permanecem consistentes.
