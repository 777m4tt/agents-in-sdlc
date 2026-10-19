---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse Mission Control, você pode ver todas as tarefas dos agentes que têm pull requests (PRs) abertos. Também pode examinar as operações executadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que também seria bom adicionar um modo claro. Como já havia trabalho para atualizar o estilo do site e adicionar a funcionalidade de alternância, pareceu lógico incluir esse recurso. Oriente o agente para que ele também adicione um modo claro.

## Revise as tarefas do agente de nuvem do Copilot

Confira o status atual de todas as tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Ela deve incluir as tarefas atribuídas ao Copilot, como:
   - atualizar a documentação da base de código.
   - gerar interfaces de programação de aplicações (APIs) para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento e revise as operações executadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores do Model Context Protocol (MCP).
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel de pull request à direita. Nele, você pode ver o PR e os arquivos alterados para acompanhar o trabalho.

## Oriente o agente de nuvem

Agora que você viu as tarefas ativas, peça ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato varia de acordo com o nome escolhido pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no Mission Control](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que o agente termine a configuração e comece o trabalho. Isso ficará evidente quando surgirem mensagens semelhantes às mostradas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, insira o seguinte prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar uma sessão ativa do Copilot destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot confirma o recebimento do prompt e o incorpora ao fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot continuará a tarefa atualizada. Ele incorporará a nova solicitação ao fluxo de trabalho depois de terminar a etapa que estava executando quando você enviou a mensagem.

O processo levará alguns minutos. Aproveite esse tempo para refletir sobre o que aprendeu e explorou até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o centro de controle para acompanhar e orientar tarefas do agente de nuvem do GitHub Copilot. Nesse Mission Control, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho realizado e redirecionar tarefas em andamento para ajustar o escopo ou fornecer mais orientações.

Você explorou os seguintes conceitos:

- usar o Mission Control e a página de agentes para acompanhar as tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, você pode [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
