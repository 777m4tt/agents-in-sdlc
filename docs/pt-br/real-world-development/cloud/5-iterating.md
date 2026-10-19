---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design iterar e implementasse recursos de acessibilidade, incluindo alternâncias para os modos de alto contraste e claro. Agora, examine as alterações de código e, se necessário, envie comentários ao Copilot para aprimorar o trabalho.

### Cenário

Como já destacamos várias vezes, os fundamentos do design de software e das práticas de desenvolvimento e operações (DevOps) não mudam com a chegada da inteligência artificial (IA) generativa. Sempre é necessário revisar o código gerado e seguir o processo habitual de DevOps. Com isso em mente, revise as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e o agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para manter o ambiente protegido. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para o código.
- O agente de nuvem é executado no GitHub Actions, onde cria um ambiente temporário e separado para trabalhar.
- Uma pessoa precisa aprovar os workflows do GitHub Actions antes de eles serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive para servidores do Model Context Protocol (MCP).

## Revise a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: a adição de documentação ao código. Você fará essa revisão pela interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao examinar o PR, talvez você veja um aviso de que o firewall bloqueou o GitHub Copilot. Isso é **esperado**, pois o acesso do Copilot a recursos externos é limitado por padrão, inclusive chamadas a servidores MCP externos. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao seu repositório em github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o pull request intitulado **Add missing documentation** ou algo semelhante.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos; você pode fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando o pull request estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui novos comentários TSDoc e outras documentações. As alterações exatas podem variar.

   Ao revisar as alterações, procure comentários TSDoc, convenções TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizadas examinados no início do Exercício 2.

6. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que há workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows devem aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

O trabalho com o Copilot em um pull request não precisa ser unilateral. Você pode mencioná-lo em comentários do pull request ou em comentários associados a trechos de código, assim como faria com outros membros da equipe. O Copilot verá esses comentários e iniciará outra sessão para responder. Como os resultados são não determinísticos, não é possível fornecer um texto prescritivo para solicitar alterações. Você pode pedir, por exemplo, que o Copilot:

- adicione cabeçalhos de comentário no início de cada arquivo de código com uma breve descrição da finalidade.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um README com uma descrição da estrutura do aplicativo Astro.

1. Adicione um comentário pedindo uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou sugira outra melhoria na documentação do código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao pull request.

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit deverá aparecer no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

6. Continue iterando até ficar satisfeito. Depois, se estiver tudo pronto, marque o PR como pronto para revisão e mescle-o à branch principal.

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Volte ao PR criado pelo Copilot para resolver a issue sobre a exibição de jogos relacionados na página de detalhes.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR intitulado **Show related games on the game details page** ou algo semelhante.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
6. Procure um indicador de que há workflows aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows devem aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** alterne para essa branch no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

10. **Opcional:** continue iterando com o agente de nuvem e solicite atualizações. Por exemplo, peça para ajustar a quantidade de jogos relacionados ou adicionar cabeçalhos de comentário e comentários TSDoc — lembre-se de que essa tarefa foi atribuída **antes** da atualização das instruções personalizadas. Para isso, adicione um comentário na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. Este PR deve incluir tanto o modo de alto contraste atribuído no Exercício 3 quanto o modo claro solicitado no Mission Control durante o Exercício 4.

1. Volte ao seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR intitulado **Add high contrast mode to website** ou algo semelhante.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o pull request terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes de alternância da interface do usuário (UI) para mudar de modo.
   - uso do armazenamento local (local storage) para persistir as preferências do usuário.
   - alterações de folhas de estilo em cascata (Cascading Style Sheets (CSS)) ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos Accessible Rich Internet Applications (ARIA), navegação por teclado etc.).
   - código JavaScript/TypeScript que gerencia a alternância entre modos.

6. Depois de revisar as alterações, volte à guia **Conversation** e role a página para baixo.
7. Procure um indicador de que há workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows devem aparecer na seção de verificações do pull request. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** alterne para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste as alternâncias dos modos de alto contraste e claro no navegador. Confirme que funcionam e que as preferências persistem após recarregar a página.

11. Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo boas práticas de acessibilidade. Se encontrar algum problema ou oportunidade de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como fez nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e se dedicar a outras tarefas. Para criar esse hábito, registre mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página da lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo destas lições, você:

- **Inspecionou os arquivos de instruções personalizadas incluídos no repositório** para observar mais tarde o efeito deles no resultado do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar alternâncias dos modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como Mission Control** para acompanhar e orientar a sessão de acessibilidade enquanto ela estava em andamento.
- **Revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração MCP diretamente no IDE.
- 💻 **[Percurso da interface de linha de comando (CLI)](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de garantia de qualidade (QA) personalizado, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribua ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- crie um agente personalizado para outro domínio.
- configure `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
