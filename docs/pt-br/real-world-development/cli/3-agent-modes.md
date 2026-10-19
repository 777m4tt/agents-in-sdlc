---
title: "Lição 3 - Modos do agente: Plan e Autopilot"
description: "Use Plan para combinar uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Mudanças maiores exigem um processo mais robusto. A interface de linha de comando (CLI) do GitHub Copilot foi desenvolvida para se integrar ao fluxo de trabalho existente da equipe e ajudar a criar as funcionalidades certas da maneira certa. Nesta lição, você começará um fluxo de desenvolvimento orientado por agente usando uma issue para criar um recurso, validar o código e verificar o comportamento antes de mesclá-lo ao projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo do recurso. Em geral, equipes podem usar branches ou pull requests (PRs) diferentes para cada tipo de alteração, mas este workshop simplifica o processo para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar o recurso no modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

Ao longo do trabalho neste recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo do Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes da implementação. Você usará o modo Plan para decidir esses detalhes e, em seguida, autorizará uma implementação delimitada com o Autopilot.

## Contexto

Introduzir agentes de programação com inteligência artificial (IA) no fluxo de desenvolvimento não muda os fundamentos — na verdade, eles se tornam ainda mais importantes. Muitos desenvolvedores seguem um processo semelhante a este:

1. Comece com uma issue registrada que descreva o que precisa ser feito.
2. Planeje o que será desenvolvido.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, mescle o código.

> [!NOTE]
> Os detalhes variam conforme a equipe e a organização, mas muitos processos seguem essas mesmas etapas.

Seguir esse processo ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelas mesmas verificações que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente propõe alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e iterando até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, prepare o Codespace, o repositório e o terminal.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o.
2. Confirme que o PR das avaliações por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número parecido). O caractere `#` permite incluir uma issue ou um pull request (PR) do GitHub no contexto.

## Planeje o recurso de filtragem

O planejamento permite definir como um recurso será implementado antes de delegar o trabalho ao Copilot. É recomendável dedicar algum tempo ao planejamento de tarefas complexas. Mude para o modo Plan e peça ao Copilot que crie um plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, insira este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot explorará o projeto e determinará uma abordagem adequada.

3. Se o Copilot fizer perguntas sobre o comportamento da filtragem, responda de acordo com suas preferências. Não há respostas certas ou erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano para revisá-lo.
6. Peça ao Copilot para revisar qualquer parte que não corresponda às decisões tomadas.

## Aprove o Autopilot

Depois de escrever e revisar o plano, implemente-o com o Autopilot. Esse modo permite que o Copilot itere até considerar o problema resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com nome semelhante na versão instalada.
2. Confirme que o indicador de modo abaixo do prompt mostra **Autopilot**.
3. Observe o Copilot seguir o plano, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes de aceitá-lo. Se o Copilot informar que faltam dependências ou que há um conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Revise o código gerado antes de mesclá-lo, como faria com qualquer outra alteração. Inspecione o código e execute o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explore a nova funcionalidade

Agora que o código parece correto, inicie o aplicativo e abra o site pela porta encaminhada do Codespaces para confirmar que ele funciona.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, por editora e pelas combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às decisões tomadas.
5. Volte ao Codespace e peça ao Copilot para parar o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou o recurso no modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, você verá como orientar a geração de código para seguir práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
