---
title: "Lição 5 - Personalize e use uma skill de verificações de qualidade"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desenvolver software envolve mais do que escrever código. Você já verificou manualmente o funcionamento e usou arquivos de instruções para aplicar os padrões da equipe. E quanto a testes, lint e às outras partes da integração contínua (CI)?

As **skills de agente** são ideais para essas tarefas. Na interface de linha de comando (CLI) do GitHub Copilot, elas ajudam o Copilot a executar operações como essas da maneira correta.

Nesta lição, você vai:

- explorar a skill existente `quality-checks`.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que o agente carrega sob demanda. Em sua forma mais simples, uma skill é uma pasta que contém o arquivo Markdown `SKILL.md`. Esse arquivo tem frontmatter com nome e descrição, uma visão geral e orientações sobre quando usar a skill. A pasta também pode conter scripts e outros recursos.

> [!NOTE]
> Uma skill não precisa ter pastas nem arquivos adicionais. A skill `quality-checks` do Tailspin Toys contém somente `SKILL.md` porque usa os comandos já existentes no projeto.

As skills podem ficar em `.github/skills` para serem compartilhadas pela equipe como um recurso do repositório ou na pasta de skills do usuário, `~/.copilot/skills`.

## Explore a skill

Explore a skill `quality-checks` criada pela equipe Tailspin Toys para executar testes unitários, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia os campos `name` e `description` no início. A descrição ajuda o Copilot a identificar quando deve usar a skill.
3. Leia as instruções e observe como elas orientam o Copilot a executar testes e lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Execute a skill antes de alterá-la

Peça ao Copilot para executar as três verificações da skill antes de mudar seu relatório.

1. Volte à conversa de filtragem no modo Interactive.
2. Use este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final.

## Personalize o relatório

Melhore o relatório para indicar o que foi executado, se teve sucesso e o que as ferramentas realmente informaram.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa. Use novamente a skill para observar a mudança no relatório.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, permita que o Copilot interaja diretamente com o site [por meio do servidor Model Context Protocol (MCP) do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
