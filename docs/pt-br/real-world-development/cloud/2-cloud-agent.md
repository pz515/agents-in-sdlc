---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Instruções personalizadas][previous-lesson] |
|:--|

É comum as organizações terem dificuldades com dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request de rascunho para revisão por uma pessoa. Assim, você pode delegar tarefas repetitivas e se concentrar em prioridades maiores.

Neste exercício, você verá como o agente de nuvem pode:

- personalizar o ambiente de geração de código.
- executar operações com segurança.
- trabalhar com issues bem delimitadas.
- receber issues atribuídas.

## Cenários

A Tailspin Toys quer resolver parte da dívida técnica. A documentação do código está ausente; como primeiro passo, a equipe quer comentários TSDoc em todas as funções exportadas.

A equipe de design também quer melhorar a descoberta de jogos. Cada página de detalhes deve mostrar outros jogos da mesma categoria para facilitar a navegação. Ainda não é necessário um acabamento final: o recurso servirá para testes de aceitação da experiência. Como há issues de prioridade maior, esse trabalho também pode ser atribuído ao agente de nuvem.

O agente trabalha de forma assíncrona, permitindo que você se concentre em outras tarefas e revise o resultado quando estiver pronto.

## Sobre o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão durante a execução ou depois de concluída. Quando a solução estiver pronta para revisão, o agente marcará você no pull request.

## A importância de instruções bem delimitadas

O GitHub Copilot não é mágico; ainda é necessário pensar e definir o escopo das tarefas. Até operações aparentemente simples podem ter complexidades. [Defina cuidadosamente o escopo das tarefas][cloud-agent-best-practices] do agente de nuvem e trate-o como um par de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo.

## Instruções personalizadas neste repositório

As lições anteriores apresentaram instruções personalizadas. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos existentes no repositório:

- `.github/copilot-instructions.md`: examine **Code standards**, principalmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Na próxima seção, atribua ao agente a issue `Code lacks documentation` e, depois, procure nos comentários TSDoc, nas convenções de TypeScript e nos cabeçalhos de arquivo as instruções que você acabou de revisar.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O agente de nuvem usa [GitHub Actions][github-actions] para configurar o ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho e pode instalar ferramentas e dependências. O projeto já inclui a configuração necessária para Node.js, dependências, navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para suas operações.
    permissions:
      # Para clonar o repositório nas etapas de configuração, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se você não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
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

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse workflow é semelhante a outros workflows do GitHub, mas tem alguns pontos importantes:

- Ele contém um job chamado `copilot-setup-steps`, executado no GitHub Actions antes do Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório para verificar se a configuração funciona.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica reduz a produtividade, dificulta a manutenção da base de código e torna mais difícil integrar novas pessoas à equipe. O Copilot pode ajudar a criar documentação em segundo plano. Neste exercício, atribua essa tarefa ao agente de nuvem e, em uma lição futura, revise o resultado.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Code lacks documentation`.
6. Em **Description**, informe:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de atribuição.

    ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Aguarde a lista de tarefas criada pelo Copilot. Isso pode levar alguns minutos enquanto ele configura o ambiente, executa o workflow descrito anteriormente, analisa o projeto e define uma abordagem.

> [!NOTE]
> A lista de tarefas do Copilot pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow descrito anteriormente), analisando o projeto e determinando a melhor abordagem para a tarefa.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do PR para baixo e localize a atualização que indica que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Explore a sessão ao vivo e acompanhe como o Copilot resolve a tarefa: ele explora o código e entende o estado do projeto, faz pausas para analisar e escolher uma abordagem e cria código.

Isso provavelmente levará alguns minutos. Um dos objetivos do agente de nuvem é executar tarefas de forma assíncrona, permitindo que você se concentre em outras atividades. Aproveite esse recurso: atribua outra tarefa ao agente e, enquanto ele trabalha, continue explorando e desenvolvendo o aplicativo.

### Adicionar jogos relacionados à página de detalhes

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Em **Description**, informe:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de atribuição.

    ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver alguns emojis 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de forma semelhante a uma pessoa desenvolvedora, não é necessário acompanhar cada etapa; revise o trabalho quando ele estiver concluído. Enquanto isso, continue para criar e usar agentes personalizados.

## Resumo e próximos passos

Você explorou o [agente de nuvem do GitHub Copilot][copilot-agents] e viu como atribuir issues para execução assíncrona. Ele pode ajudar a resolver dívida técnica, criar recursos ou migrar código entre frameworks. Você:

- personalizou o ambiente de geração de código.
- aprendeu sobre a execução segura de operações.
- viu a importância de issues bem delimitadas.
- atribuiu issues ao Copilot.

Agora, volte a atenção para a criação e o uso de agentes personalizados. O agente de nuvem também pode usar [servidores MCP][cloud-agent-mcp] e instruções personalizadas.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próximo exercício: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
