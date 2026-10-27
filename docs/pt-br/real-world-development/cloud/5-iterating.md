---
title: "Exercício 5: Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência do usuário e adicionar recursos. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados e implementasse recursos de acessibilidade, como controles para alternar entre os modos de alto contraste e claro. Agora, revise as alterações e, se necessário, peça ao Copilot que as aprimore.

### Cenário

Os fundamentos do desenvolvimento de software e DevOps não mudam com a IA generativa. Revise o código gerado e siga o processo normal de DevOps. Antes de envolver a equipe, examine as sugestões do Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade.

## Segurança e o agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão direta, há limites de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de escrita **somente** à branch que usa para o código.
- O agente executa o trabalho em um ambiente efêmero separado no GitHub Actions.
- Uma pessoa precisa aprovar os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), inclusive para servidores MCP.

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem, que adiciona documentação ao código.

> [!NOTE]
> O PR pode mostrar um aviso de que o Copilot foi bloqueado por um firewall. Isso é esperado, pois o acesso a recursos externos, inclusive servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o PR com um título semelhante a **Add missing documentation** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o PR terá a marca **[WIP]**. Aguarde a conclusão. Isso pode levar alguns minutos; aproveite o tempo para fazer uma pausa ou refletir sobre o que aprendeu.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, incluindo os novos comentários TSDoc e a documentação adicionada. As alterações exatas podem variar. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de arquivo; eles seguem as instruções personalizadas que você examinou no início do Exercício 2.
6. Quando terminar a revisão e tudo parecer correto, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows aparecerão na seção de verificações do PR. Aguarde a conclusão e confirme que as verificações do projeto para o aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR ou comentários de linha no código. Ele verá o comentário e iniciará outra sessão para atender à solicitação. Por exemplo:

- adicionar cabeçalhos que descrevam cada arquivo.
- incluir comentários TSDoc em arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com outra pessoa da equipe. Use uma das ideias acima ou peça outra melhoria de documentação que gostaria de ver na base de código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, verifique se há um novo commit no PR.
5. Selecione a guia **Files changed** e revise as alterações.

Continue iterando até que o resultado esteja pronto. Quando estiver satisfeito, converta o PR de rascunho em pronto para revisão e faça o merge na branch principal.

![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page** ou outro título mais descritivo.
4. Selecione a guia **Files changed** e revise o código gerado.
5. Quando terminar a revisão e tudo parecer correto, volte à guia **Conversation** e role a página para baixo.
6. Verifique se há um indicador de workflows aguardando aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows aparecerão na seção de verificações do PR. Aguarde a conclusão e confirme que as verificações do projeto para o aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** alterne para essa branch no Codespace para testar manualmente o recurso de jogos relacionados. Abra o terminal do Codespace e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Assim como no PR anterior, você pode iterar com o agente de nuvem solicitando alterações. Por exemplo, peça ao Copilot que ajuste a quantidade de jogos relacionados ou adicione cabeçalhos e comentários TSDoc — lembre-se de que essa issue foi atribuída **antes** da atualização das instruções personalizadas. Para isso, adicione um comentário na guia **Conversation**; o Copilot verá o comentário e iniciará uma nova sessão.

## Revisar os recursos de acessibilidade

Por fim, revise o PR do agente personalizado de acessibilidade. Ele deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website** ou outro título mais descritivo.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando na tarefa, o PR terá a marca **[WIP]**. Aguarde a conclusão, o que pode levar alguns minutos.

4. Selecione a guia **Files changed** e revise o código gerado.
5. Examine a implementação, prestando atenção especial a:
   - componentes de interface dos controles para alternar entre os modos.
   - uso do armazenamento local para manter as preferências.
   - alterações de CSS e estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a alternância entre modos.
6. Quando terminar a revisão e tudo parecer correto, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de workflows aguardando aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows aparecerão na seção de verificações do PR. Aguarde a conclusão e confirme que as verificações do projeto para o aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** alterne para essa branch no Codespace para testar manualmente os recursos de acessibilidade. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Em seguida, inicie o aplicativo e teste no navegador os controles dos modos de alto contraste e claro. Confirme se funcionam e se as preferências persistem após recarregar a página.

Se notar problemas de acessibilidade ou oportunidades de melhoria, mencione **@copilot** em um comentário e solicite uma atualização.

## Exercício opcional: continuar delegando

O agente de nuvem é útil quando você pode delegar itens reais do backlog e se concentrar em outras tarefas. Para praticar, crie mais issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse para apoiadores na página de detalhes de um jogo.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Você:

- inspecionou as instruções personalizadas incluídas no repositório.
- atribuiu issues ao agente de nuvem e observou a configuração, o planejamento e a execução assíncrona.
- criou e usou um agente personalizado de acessibilidade para adicionar os modos de alto contraste e claro.
- acompanhou e orientou a sessão de acessibilidade na página de agentes do Copilot.
- revisou e aprimorou pull requests do agente de nuvem, mencionando `@copilot` e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar, explore outros percursos:

- **[Percurso do VS Code](../../vscode/)** — use o modo de agente do Copilot Chat e a integração com MCP no IDE.
- **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com Plan e Autopilot, skills, um agente de QA, MCP do Playwright, Agent Merge e a integração opcional `/delegate`.

No seu repositório, experimente atribuir uma issue de refatoração ou cobertura de testes ao agente de nuvem, criar outro agente personalizado ou configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas do workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Exercício anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
