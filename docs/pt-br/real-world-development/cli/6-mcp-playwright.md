---
title: "Lição 6 - Valide a funcionalidade com o Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar a filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já vimos, desenvolver envolve mais do que escrever código. Também precisamos trabalhar com dados e serviços externos e disponibilizar automações adicionais para o Copilot. Os servidores do Model Context Protocol (MCP) ampliam as ferramentas e os serviços disponíveis na interface de linha de comando (CLI) do GitHub Copilot.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor MCP do Playwright, caso ele ainda não esteja disponível.
- pedir ao agente para controlar um navegador e explorar o recurso de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface do usuário exige interagir com ela. Você permitirá que o Copilot use o site como uma pessoa usuária para automatizar a verificação das alterações e aumentar a confiança de que o recurso funciona como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de inteligência artificial (IA) se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

Um servidor MCP funciona como uma ponte entre o agente de IA e as ferramentas ou serviços externos. Cada servidor oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP populares:

- **[Servidor MCP do GitHub][github-mcp]**: oferece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegador com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro MCP][mcp-registry] para facilitar a descoberta e as contribuições para esse ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP:

   ```plaintext
   /mcp add
   ```

3. No campo de nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o comando a seguir na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo do MCP.

## Peça ao Copilot para explorar o recurso com o Playwright

Você já confirmou manualmente que a funcionalidade funciona. Agora use o servidor MCP do Playwright para repetir a verificação.

1. Use este prompt para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente, o Copilot identifica sozinho quando deve usar o servidor MCP. Ainda assim, indicar a ferramenta pode tornar os resultados mais consistentes e economizar tokens.

2. Observe as etapas executadas pelo Copilot no navegador para confirmar o funcionamento do recurso.
3. Leia o relatório e confirme se tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, encerrará o servidor e apresentará um relatório.

## Resumo e próximos passos

Você usou o servidor MCP do Playwright para explorar o recurso em um navegador real a partir do Copilot CLI. Nesta lição, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível.
- pediu ao agente para controlar um navegador e explorar o recurso de filtragem.

Em seguida, você [criará um agente personalizado de QA][next-lesson] que reúne a skill e as ferramentas de navegador.

## Recursos

- [O que é MCP e por que todos estão falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
