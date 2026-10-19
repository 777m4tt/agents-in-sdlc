---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial (IA) generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou houver informações de contexto que o Copilot deva conhecer —, disponibilize-as. Há várias maneiras de compartilhar contexto específico com o Copilot. Uma das principais é usar [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos com escopo por caminho.
- gerar a primeira parte dos dados de filtragem (um helper de editoras) com as instruções *atuais* em vigor.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão.
- fazer commit das instruções e da primeira parte da filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode divergir de alguns dos padrões definidos. O Copilot é não determinístico. O objetivo é observar a *tendência* de mudança de comportamento depois de atualizar as instruções, e não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem orientações e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa ter testes unitários.
- A interface do usuário deve usar modo escuro e ter um visual moderno.
- A documentação do código deve ser escrita em comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve explicar sua finalidade.

Com arquivos de instruções, você dará ao Copilot as informações necessárias para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Você pode especificar convenções de programação, bibliotecas e comentários desejados. Também pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações de nível de projeto relevantes para a maioria das solicitações enviadas por chat ou pela interface de linha de comando (CLI), como a stack, uma visão geral do projeto, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md` pode conter orientações para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface do usuário ou testes unitários.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados apenas para gerar código no Copilot Chat — não para preenchimento de código nem sugestões da próxima edição.
>
> O Copilot Chat, a interface de linha de comando do Copilot (CLI) e o agente de nuvem do Copilot usam arquivos de instruções tanto no nível do repositório quanto `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros formatos][custom-instructions-support], incluindo `AGENTS.md` e `CLAUDE.md`.

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre como criar arquivos de instruções está além do escopo deste workshop. Os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações de nível de projeto, como a descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de programação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e orientações claras. Dê exemplos de como o código deve — e não deve — ser escrito.

Não há uma única maneira correta de criar arquivos de instruções, assim como não há uma única maneira de usar IA. A experimentação ajudará você a descobrir o que funciona melhor no projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto abrangente de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá orientações para vários tipos de tarefas, incluindo [atualizações de interface do usuário (UI)][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece esse recurso de forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição correspondente ao ambiente explicará quando isso for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos no repositório — há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Há instruções para arquivos Astro, para a camada de dados Drizzle, para testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início. Ele define um glob, relativo à raiz do repositório, que determina os arquivos abrangidos. Neste caso, arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, são incluídos.
5. Observe as orientações específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Isso permite dividir instruções extensas em arquivos menores e reutilizáveis, além de indicar ao Copilot exemplos a seguir ao gerar código. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Crie uma branch para as alterações

Siga boas práticas e crie uma branch para as alterações.

1. Volte ao Codespace usado no exercício anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e alterne para uma nova branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de iniciar os exercícios a seguir, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam entre exercícios; cada etapa indicará as opções relevantes.

Para observar o efeito das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Mais tarde, você o atualizará e enviará o mesmo prompt novamente.

1. Feche as guias de editor abertas em exercícios anteriores para que o Copilot receba apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o helper deve ficar.
3. Na lista de agentes da visualização Chat, selecione **Agent** para que o Copilot possa aplicar alterações nos arquivos.

   ![Captura de tela da seleção de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações de código, muitas vezes abrangendo o arquivo helper e os testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras. Isso vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observe que o código proposto **não tem** comentários TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, ele pode adicionar comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo continua sendo observar a melhora da *consistência* depois de atualizar as instruções.

## Adicione um padrão ao repositório

O arquivo `.github/copilot-instructions.md` fornece informações de nível de projeto ao Copilot. Documente nele os padrões de programação do repositório para orientar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões do projeto, mas ainda não tem uma regra de documentação no código, razão pela qual o helper gerado não tinha comentários.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes. Elas instruem o Copilot a incluir cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, arquivos de instruções podem ficar no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicas. O arquivo do repositório é o local adequado para padrões válidos em todo o projeto, como a regra de comentários que você acabou de adicionar.

## Execute o prompt novamente e observe a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto da conversa.
2. Selecione `src/lib/publishers.ts` para direcionar o Copilot ao arquivo correto.
3. Envie **o mesmo prompt** usado anteriormente:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código que será usado no próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são controlados como qualquer outro arquivo do repositório. Faça commit das alterações e envie a branch.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as alterações nas instruções e no helper estão presentes:

   ```bash
   git status
   ```

3. Adicione ao stage e faça commit das instruções e de todos os arquivos alterados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Com a branch enviada, crie um pull request associado à issue sobre o padrão de documentação do backlog. O Copilot pode localizar a issue, criar um PR que a feche e mesclá-lo usando as ferramentas do GitHub já conectadas ao projeto.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub em seu nome. O modelo do projeto já configura as ferramentas do GitHub no espaço de trabalho, então não é necessário configurá-las. Na primeira vez que o Copilot usar uma ferramenta, o VS Code poderá pedir que você entre no GitHub. Siga as instruções para permitir o acesso. Em um exercício posterior, você verá como essa conexão funciona por meio do Model Context Protocol (MCP).

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado na lista de agentes.
3. Peça ao Copilot que encontre a issue sobre a atualização dos arquivos de instruções e crie um PR a partir da branch atual, descrevendo as atualizações das instruções e a base do helper de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando solicitado a permitir ações do GitHub em seu nome, revise o comando e selecione **Allow** conforme apropriado.
6. Depois que o PR for criado, peça ao Copilot que o mescle e retorne sua branch local a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado a permitir ações do GitHub e comandos de shell em seu nome, revise o comando e selecione **Allow** conforme apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit das instruções e da base do helper em `main` para que o próximo exercício possa usá-los.

Em seguida, você aplicará essas instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona um recurso novo à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
