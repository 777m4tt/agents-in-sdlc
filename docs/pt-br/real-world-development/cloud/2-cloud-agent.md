---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que quase todas as organizações enfrentem algum nível de dívida técnica. Pode ser uma vulnerabilidade de segurança não resolvida, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um pull request (PR) em rascunho para revisão de um desenvolvedor. Isso permite delegar tarefas repetitivas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais amplas.

Neste exercício, você explorará como usar o agente de nuvem do Copilot para:

- personalizar o ambiente usado para gerar código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues com escopo bem definido.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem alguns itens de dívida técnica para resolver. Os prestadores de serviço que criaram a primeira versão do site deixaram a documentação em um estado insatisfatório — na verdade, você verá que ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem navegando. Ainda não é necessário um resultado refinado; a equipe quer algo que possa usar para testes de aceitação da experiência do usuário (UX). Essa funcionalidade é um bloqueio no momento, mas há outras issues de prioridade mais alta.

Esses são exemplos de tarefas que podem acabar perdendo prioridade e que são adequadas para delegar ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto o desenvolvedor se concentra em outras tarefas e depois volta para revisar o trabalho e confirmar se está correto.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de maneira semelhante a um desenvolvedor. Assim como no trabalho com uma pessoa desenvolvedora, há várias formas de atribuir o trabalho, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request (PR) em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode inspecionar a sessão do Copilot enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot: você ainda precisa pensar. Até tarefas aparentemente simples podem ser complexas quando examinadas em detalhe. Por isso, [defina com cuidado o escopo das tarefas do agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com inteligência artificial (IA): trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Em exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções incluídos neste repositório, sem alterá-los, para perceber o efeito deles mais tarde.

Abra os seguintes arquivos na interface web do GitHub ou em um Codespace que já esteja em execução para observar as instruções do repositório:

1. Abra `.github/copilot-instructions.md` e revise a seção **Code standards**, principalmente as expectativas para convenções TypeScript e comentários TSDoc.
2. Abra `.github/instructions/unit-tests.instructions.md` e observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe os comentários TSDoc, as convenções TypeScript e os cabeçalhos de comentário no pull request resultante. Esses elementos vêm dos arquivos de instruções. Você verá esse ponto novamente ao revisar PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

Criar código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. Isso também se aplica às tarefas atribuídas ao Copilot, que trabalha de forma semelhante a um engenheiro de software (SWE).

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo criando um [workflow de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, que será executado antes de o agente começar. Assim, ele terá acesso às ferramentas de desenvolvimento e dependências necessárias. O arquivo já foi configurado para este laboratório: ele disponibiliza Node.js, as dependências do projeto, os binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (agora todo o aplicativo usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e popula o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para confirmar que ele funciona sem esperar o Copilot iniciá-lo.

## Adicione a documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novos desenvolvedores. O Copilot é eficaz na criação de documentação, por isso esta é uma issue adequada para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, insira:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o novo pull request (PR), cujo título será semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está criando o ambiente — executando o workflow mencionado anteriormente —, analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request até encontrar uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve o problema. Ele explora o código, entende o estado atual, faz pausas para planejar e tomar decisões e cria código.

O processo pode levar vários minutos. Uma das principais vantagens do agente de nuvem do Copilot é poder executar tarefas de forma assíncrona, deixando você livre para trabalhar em outras coisas. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, voltando nossa atenção para a criação de funcionalidades no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: enquanto ele se concentra em uma tarefa, você pode cuidar de outra. Mesmo que adicionar uma seção de jogos relacionados para a equipe de design não demore muito, esse tempo pode ser dedicado a outras tarefas. Atribua esse trabalho ao agente de nuvem do Copilot.

1. Volte ao seu repositório em github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de criação de issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, insira:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver um emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de maneira semelhante a um SWE, não é necessário acompanhá-lo continuamente; revise o resultado quando terminar. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para execução assíncrona. Você pode usá-lo para resolver dívida técnica, criar funcionalidades ou ajudar a migrar código de um framework para outro.

Você explorou os seguintes conceitos:

- personalizar o ambiente usado para gerar código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues com escopo bem definido.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, você pode se concentrar em criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e tem acesso a instruções personalizadas, exploradas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
