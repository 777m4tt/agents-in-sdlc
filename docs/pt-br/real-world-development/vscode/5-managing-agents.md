---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que ocorrem. Você pode revisar cada diff no chat, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts complementares para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design solicita também uma alternância para o modo claro, para usuários que consideram o tema padrão escuro demais. Em vez de iniciar uma nova conversa, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: os arquivos que explora, os comandos que executa e as edições que propõe. Cada edição aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra o chat para acompanhar cada chamada de ferramenta. Você verá leituras de arquivos, edições e comandos de terminal que ele solicitar para executar.
3. Quando o agente propuser uma edição, o diff aparecerá no chat:
   - selecione **Keep** para aceitar a alteração.
   - selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todo o conjunto de arquivos modificado pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando no terminal, o agente pausará e mostrará uma solicitação **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Ele deverá apresentar uma mensagem final resumindo as alterações.

## Oriente o agente para adicionar o modo claro

Com o modo de alto contraste implementado, amplie a mesma conversa para adicionar uma alternância para o modo claro. Como você continuará na sessão existente, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição à medida que aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface das alternâncias de uma maneira que você não quer —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem complementar para esclarecer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes ("simplify the CSS — use CSS variables instead of duplicating selectors"), corrigir a abordagem ("don't add a third toggle, integrate light mode into the existing settings panel") ou ampliar o escopo ("also add keyboard shortcuts for both toggles").

## Revise o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e nos testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme que foram aplicadas as práticas de acessibilidade do agente personalizado: atributos Accessible Rich Internet Applications (ARIA), navegação por teclado, HyperText Markup Language (HTML) semântico e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare as alterações para commit e faça commit pelo painel Source Control. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou as edições conforme surgiam, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho de um agente em tempo real.
- aceitou e rejeitou edições individuais nos arquivos.
- orientou uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local ao longo deste laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
