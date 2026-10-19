---
title: "Exercício 3 - Teste seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Em seguida, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis para o Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que use o servidor para testar manualmente a filtragem no navegador.
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite isso: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama que mostra o funcionamento interno do modo de agente e como ele interage com contexto, modelos de linguagem de grande porte (LLMs) e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de inteligência artificial (IA) se comuniquem com ferramentas e serviços externos. Com MCP, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome por meio de ferramentas.

Essas ferramentas e recursos são acessados por um servidor MCP, que faz a ponte entre o agente de IA e os serviços externos. O servidor MCP gerencia a comunicação entre o agente e ferramentas externas, como interfaces de programação de aplicações (APIs) existentes ou ferramentas locais, por exemplo, pacotes npm. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP populares:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios do GitHub. O agente pode criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: oferece recursos de automação de navegador com o Playwright. O agente pode navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP que oferecem acesso a diversas ferramentas e recursos. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições para esse ecossistema.

> [!CAUTION]
> Em relação à segurança, trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores em que confia e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Confirme que já existe um servidor `github` configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

A entrada `github` vem do modelo do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não é necessário instalar nada localmente. O VS Code se conecta pelo Hypertext Transfer Protocol (HTTP), e você autentica no GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicione o servidor MCP do Playwright

Adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece um navegador que o Copilot pode controlar, necessário para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, de modo que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` instrui o Playwright a executar o navegador sem uma janela visível, o que é necessário em um Codespace, onde não há área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, portanto o navegador necessário geralmente já está instalado. Se o Copilot informar que o navegador está ausente, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e autorize o servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisar das ferramentas de um servidor, o VS Code o inicia e pede confirmação de que você confia nele.

1. Em `.vscode/mcp.json`, aparece uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e confirme que confia nele para permitir a inicialização.

Iniciar o servidor manualmente é opcional. Se você não fizer isso, o VS Code o iniciará e pedirá sua autorização na primeira vez que o Copilot precisar das ferramentas, na próxima etapa.

## Teste a funcionalidade de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e resumir o que observou. É uma maneira rápida de confirmar que o recurso funciona como esperado sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. Assim, o agente raciocina sobre elementos estruturados e rotulados — botões, links e itens de lista — da mesma forma que uma tecnologia assistiva. Uma verificação funcional rápida também oferece uma checagem básica de acessibilidade.

Com o servidor conectado, verifique se o aplicativo está em execução e peça ao Copilot que teste o recurso de filtragem recém-criado.

1. Se o aplicativo não estiver em execução em `http://localhost:4321`, inicie-o com `npm run dev`.
2. Envie ao Copilot este prompt:

   ```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
   ```

3. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e repita o teste.

O Copilot abrirá um navegador pelo servidor MCP do Playwright, executará cada etapa e relatará o que observou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas complementares ou peça que corrija o código antes de abrir um pull request.

> [!NOTE]
> Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Depois de confirmar que o recurso funciona, publique a branch `filtering-vscode` para que a equipe possa revisar as alterações.

1. Abra o painel **Source Control** no Codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações para commit selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, peça que ele crie o PR. Ele pode localizar a issue correspondente e associá-la ao PR.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Na lista de agentes, confirme que **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute operações. Na primeira vez que usar uma ferramenta do GitHub, talvez seja necessário autenticar. Siga as instruções para permitir o acesso.
5. Observe o Copilot procurar nas issues, localizar a correta e criar o PR.
6. Selecione o link criado pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas disponíveis para o Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou o servidor para testar manualmente o recurso de filtragem no navegador antes de publicá-lo.
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
