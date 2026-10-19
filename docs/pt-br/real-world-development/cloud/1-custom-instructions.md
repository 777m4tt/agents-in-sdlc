---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial (IA) generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou houver informações de contexto que o Copilot deva conhecer —, disponibilize-as. Os [arquivos de instruções][instruction-files] fornecem essas orientações para que o Copilot entenda não só *o que* você quer, mas também *como* deve fazê-lo.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos com escopo por caminho.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos do VS Code e da interface de linha de comando (CLI), aqui você não executará um prompt *antes e depois*. O agente de nuvem do Copilot trabalha de forma assíncrona nas issues do GitHub, então é mais difícil demonstrar a influência lado a lado em tempo real. Você observará o efeito do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente.

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

- `.github/copilot-instructions.md` é um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações de nível de projeto relevantes para a maioria das solicitações enviadas por chat ou pela CLI, como a stack, uma visão geral do projeto, boas práticas e outras orientações globais.
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
> Todo projeto que usa o GitHub Copilot deve ter um conjunto abrangente de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá orientações para vários tipos de tarefas, incluindo [atualizações de interface do usuário][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece esse recurso de forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` na CLI do Copilot. A lição correspondente ao ambiente explicará quando isso for relevante.
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

## Adicione um padrão ao repositório

Agora, adicione uma regra válida para todo o projeto que exija comentários TSDoc e um cabeçalho de comentário no nível do arquivo. O agente de nuvem usará essa orientação ao trabalhar nas issues que você atribuir a ele mais adiante.

1. No terminal do Codespace, crie e alterne para uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

O arquivo `.github/copilot-instructions.md` fornece informações de nível de projeto ao Copilot. Documente nele os padrões de programação do repositório para orientar as sugestões de código.

2. Abra novamente `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões do projeto, mas ainda não tem uma regra de documentação no código, razão pela qual o helper gerado não tinha comentários.
4. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes. Elas instruem o Copilot a incluir cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, arquivos de instruções podem ficar no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicas. O arquivo do repositório é o local adequado para padrões válidos em todo o projeto, como a regra de comentários que você acabou de adicionar.

## Faça commit, envie e mescle a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. No próximo exercício, ao atribuir uma issue ao Copilot, ele criará uma branch a partir de `main`. Portanto, suas alterações nas instruções precisam estar em `main` para que o agente de nuvem as use.

1. Adicione as alterações ao stage e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. Em github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça a mesclagem. O agente de nuvem lerá essas instruções ao trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a etapa de criação da branch e fazer commit diretamente em `main`. A branch foi incluída para que o workshop reflita uma prática comum em projetos reais.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e adicionou um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`. Você verá a influência desse padrão nos pull requests produzidos pelo agente de nuvem ao longo deste percurso.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
