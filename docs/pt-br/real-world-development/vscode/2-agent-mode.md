---
title: "Exercício 2 - Adicione funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e tarefas como executar testes. O fluxo de trabalho de desenvolvimento geralmente envolve localizar todos os arquivos necessários, fazer as alterações, executar testes, depurar, descobrir qual arquivo foi esquecido, fazer outra atualização e assim por diante.

É para esse tipo de tarefa que existe o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar de forma mais autônoma no ambiente de desenvolvimento integrado (IDE). Ele se comporta de maneira semelhante a um desenvolvedor: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos ver como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE e permite ciclos rápidos de feedback e interação. O agente de nuvem funciona de forma assíncrona, como uma pessoa da equipe, interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo de agente do GitHub Copilot implementa novos recursos na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você já adicionou um helper de editoras. Agora, concluirá as alterações restantes na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site Tailspin Toys

Antes de alterar qualquer coisa, explore o site Tailspin Toys para conhecer a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema de desenvolvimento de software. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza as páginas como HyperText Markup Language (HTML) estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do mapeador objeto-relacional (ORM) Drizzle. Não há uma interface de programação de aplicações (API) de back-end separada nem um framework de interface do lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`; a interatividade é implementada com pequenos elementos `<script>` do Astro, com escopo definido e usando APIs padrão do Document Object Model (DOM).

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o servidor de desenvolvimento Astro. Execute-a no GitHub Codespace seguindo estas etapas:

1. Volte ao Codespace. Depois do exercício anterior, você deve estar na branch `main`.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e alterne para uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um aviso com a URL, semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Para abrir o site, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse endereço é um túnel privado para o Codespace, que agora hospeda o servidor web.

### Explore o site

Com o site em execução, explore as funcionalidades atuais:

- **Home Page**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Game Details Page**: ao selecionar um jogo, você acessa uma página com mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de iniciar os exercícios a seguir, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam entre exercícios; cada etapa indicará as opções relevantes.

A implementação inicial do site funciona, mas queremos adicionar recursos. Comece explorando o backlog. Ao criar o projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre esses itens.

1. Na lista de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos no terminal e chama ferramentas de forma autônoma.

   ![Captura de tela da seleção de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em detrimento de outro. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código fossem relativamente consistentes. Como os modelos de linguagem de grande porte (LLMs) são probabilísticos, as sugestões podem ser diferentes das mostradas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem de grande porte (LLMs), o Copilot pode usar um comando do Model Context Protocol (MCP) diferente, mas ainda deverá concluir a tarefa.

3. Peça ao Copilot que mostre o backlog de issues usando este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens com base no que considera mais útil para o usuário.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que dará contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a adição de funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais sobre como adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, capacidade de teste e acessibilidade.
   - links para instruções específicas de tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Assim, é possível reutilizar orientações, dividir instruções complexas em partes menores e fornecer exemplos e modelos.

## Implemente a funcionalidade de filtragem

Para concluir a filtragem, será necessário fazer pelo menos três alterações distintas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`).
- adicionar ou atualizar os testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar as alterações à base de código. O modo de agente do Copilot pode fazer essas tarefas. Adicione a funcionalidade:

1. Continue na conversa atual do Copilot ou inicie outra selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como o arquivo de instruções.

   ![Captura de tela da seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista de agentes da visualização Chat.

   ![Captura de tela da seleção de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade: helpers da camada de dados, interface e testes. Em seguida, ele começará a alterar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você verá um deles para cada alteração; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e até falhas em alguns testes durante o processo. Isso é esperado: o Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do modo de agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado em busca de possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de inteligência artificial (IA).

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, você usou o modo de agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Você aprendeu como:

- o modo de agente do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Agora, [teste seu recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra opcional — implemente paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Usando o que aprendeu neste exercício, peça ao Copilot que implemente a paginação no site. Ao planejar a implementação:

1. Siga as boas práticas existentes, incluindo o uso dos arquivos de instruções.
2. Decida como a paginação deve funcionar: permita que o usuário escolha o tamanho da página ou defina um valor fixo.
3. Forneça ao Copilot as orientações necessárias para obter a implementação desejada.
4. Itere com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
