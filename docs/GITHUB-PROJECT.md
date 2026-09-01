# GitHub Issues e Project do AutoJob

## Objetivo

O GitHub Project organiza a execução incremental do AutoJob sob o limite diário do Codex. `TASKS.md` é o backlog versionado; cada unidade selecionada para execução deve possuir uma GitHub Issue correspondente e manter o mesmo ID.

Uma Issue representa uma única entrega verificável. Cada sessão do Codex deve trabalhar preferencialmente em apenas uma Issue e deve parar ao concluí-la ou bloqueá-la. O agente não escolhe nem inicia automaticamente a próxima Issue.

## Identidade e metadados das Issues

Título recomendado: `[AJ-E##-###] Título da task`.

Campos obrigatórios:

| Campo | Opções | Uso |
| --- | --- | --- |
| Epic | E0 a E11 | Agrupa a Issue pelo resultado de roadmap. |
| Priority | P0, P1, P2 | Ordena urgência sem substituir dependências. |
| Size | XS, S, M, L | Estima se a Issue cabe em uma sessão focada. |
| Type | Feature, Infra, Test, Docs, Security, Bug | Classifica a natureza principal da entrega. |

Além dos campos do Project, a Issue deve conter ID, objetivo único, dependências, escopo, fora de escopo, critérios de aceite, verificação e notas de segurança/custo.

### Priority

- **P0:** bloqueia fundação/vertical slice ou implementa guardrail obrigatório.
- **P1:** necessário ao MVP depois que o fluxo principal estiver estabelecido.
- **P2:** expansão, otimização ou componente opcional/pós-MVP.

Prioridade nunca permite ignorar uma dependência.

### Size

- **XS:** mudança isolada e de baixa incerteza.
- **S:** entrega pequena com testes e documentação em uma sessão.
- **M:** exige revisão de granularidade antes de `Ready`; divida se houver resultados independentes.
- **L:** proibida em `Ready`; deve ser dividida.

## Status do Project

| Status | Significado | Regra de entrada | Regra de saída |
| --- | --- | --- | --- |
| Backlog | Task conhecida, ainda não selecionada ou bloqueada. | Issue criada/importada, ou devolvida por dependência/bloqueio. | Vai para `Ready` somente após cumprir todo o gate de prontidão. |
| Ready | Task livre de dependências e pronta para uma sessão Codex. | Metadados completos, dependências em `Done`, aceite verificável, tamanho adequado e capacidade disponível. | Vai para `In Progress` apenas quando explicitamente selecionada para execução. |
| In Progress | Issue atual com trabalho autorizado em andamento. | Agente/colaborador assume a Issue; preferencialmente uma por sessão. | Vai para `Review` com implementação e evidência completas, ou volta a `Backlog` se bloqueada. |
| Review | Entrega concluída aguardando revisão/CI/merge. | PR associado, critérios marcados e verificação registrada. | Vai para `Done` após aprovação, CI aplicável e merge; volta a `In Progress` se houver mudanças requeridas. |
| Done | Critérios aceitos e alteração integrada. | PR merged ou entrega documental aceita com evidência equivalente. | Estado terminal; reabertura exige justificativa ou nova Issue de bug. |

## Gate de prontidão (`Backlog` → `Ready`)

Uma Issue só pode entrar em `Ready` quando todos os itens abaixo forem verdadeiros:

- todas as dependências listadas estão em `Done`;
- ID, título, Epic, Priority, Size e Type estão preenchidos;
- há um único objetivo e critérios de aceite observáveis;
- escopo e fora de escopo são explícitos;
- verificação esperada está definida;
- riscos de segurança e custo foram avaliados;
- o tamanho é `XS` ou `S`, ou uma `M` foi revisada e comprovadamente indivisível;
- existe capacidade na coluna `Ready`.

Se qualquer dependência reabrir ou surgir novo bloqueio, a Issue volta para `Backlog` e recebe uma nota/label de bloqueio. Não mantenha trabalho bloqueado em `Ready`.

## Limites de trabalho em andamento

- `Ready` deve conter **no mínimo 1 e no máximo 3 Issues** quando houver tasks elegíveis.
- Não promova tasks apenas para preencher a coluna; ausência de task elegível é aceitável.
- Uma sessão Codex trabalha preferencialmente em **uma única Issue**.
- O agente não inicia uma segunda Issue na mesma sessão só porque terminou cedo.
- Novas Issues entram primeiro em `Backlog`, nunca diretamente em `In Progress`.
- Issues `L` não entram em `Ready`.

O limite de uma a três opções reduz planejamento especulativo e desperdício do orçamento diário do Codex.

## Seleção da próxima Issue

Quando o usuário solicitar uma nova implementação:

1. Considere somente Issues em `Ready`.
2. Confirme novamente que suas dependências estão em `Done`.
3. Prefira menor tamanho; entre tamanhos iguais, use Priority e a ordem do roadmap.
4. Trabalhe apenas na Issue explicitamente escolhida.
5. Ao concluir, mova-a para `Review` e pare.

A primeira Issue de implementação elegível após a TAREFA 002 é **AJ-E00-002 — Registrar decisão da toolchain TypeScript**. Ela depende apenas da fundação documental concluída e não implementa funcionalidade do AutoJob.

## Fluxo Issue → Pull Request → Done

1. Criar a Issue com `.github/ISSUE_TEMPLATE/task.md` ou `bug.md`.
2. Preencher os campos e manter a Issue em `Backlog` até o gate de prontidão.
3. Promover para `Ready` respeitando dependências e o limite de 1–3.
4. Ao iniciar a sessão autorizada, mover para `In Progress`.
5. Abrir PR com `.github/pull_request_template.md` e relacionar a Issue (`Closes #...`).
6. Registrar comandos e evidências; mover para `Review`.
7. Após aprovação, CI e merge, mover para `Done`.
8. Parar. A próxima Issue requer seleção/instrução explícita.

## Regras para bugs

- Um bug documenta divergência reproduzível em relação ao PRD/SDD, ADR ou critério aceito.
- A correção deve ser mínima e incluir teste de regressão quando aplicável.
- Refatorações e melhorias descobertas durante o diagnóstico viram Issues separadas.
- Bugs seguem os mesmos gates de dependência, WIP, revisão, segurança e custo.
- Um bug de segurança pode ser P0, mas prioridade não autoriza publicar detalhes sensíveis.

## Manutenção do backlog

- Ao criar Issues a partir de `TASKS.md`, não altere IDs nem duplique a mesma task.
- Mudança material de objetivo/dependência exige atualização de `TASKS.md` na mesma entrega.
- Tasks amplas devem ser divididas antes de `Ready`, com dependências e rastreabilidade ajustadas.
- O Project reflete o estado operacional; `TASKS.md` preserva a definição versionada.
- A ordem do Project não substitui `ROADMAP.md` nem as decisões em ADR.
