---
title: "Exercício 3 - Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Exercício anterior: Modo de agente][previous-lesson] · [Próximo exercício: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Configure o **servidor MCP do Playwright** para que o Copilot controle um navegador real, teste o recurso e, depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que teste manualmente a filtragem no navegador.
- publicar sua branch e abrir um pull request.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica mais poderoso quando pode usar ferramentas externas. O Model Context Protocol (MCP) é um padrão que permite ao Copilot se comunicar com essas ferramentas e serviços.

![Diagrama da interação do modo de agente com contexto, LLM e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA acessem informações atualizadas e executem ações em nome do usuário por meio de recursos e ferramentas. Um servidor MCP conecta o agente às ferramentas e serviços externos, como APIs e pacotes locais.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests.
- **[Servidor MCP do Playwright](https://github.com/microsoft/playwright-mcp)**: permite automatizar o navegador, navegar por páginas, preencher formulários e selecionar botões.

O GitHub mantém um [registro de MCP](https://github.com/mcp) para facilitar a descoberta de servidores e a contribuição.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. O servidor `github` já deve estar configurado:

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

Essa configuração vem do modelo do projeto e permitiu que o Copilot lesse o backlog na lição anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto não é necessário instalá-lo localmente. O VS Code conecta-se por HTTP e solicita autenticação do GitHub na primeira utilização.

## Adicionar o servidor MCP do Playwright

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável.

1. No `.vscode/mcp.json`, adicione `playwright` ao lado de `github`:

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

Ao contrário do servidor remoto `github`, `playwright` é local: o VS Code o inicia com `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, necessário em um Codespace sem área de trabalho gráfica.

> [!NOTE]
> O projeto já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP sob demanda, quando o Copilot precisa das ferramentas, e solicita confirmação de confiança.

1. Em `.vscode/mcp.json`, selecione **Start** acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code pedir que você confie no servidor, revise a configuração e confirme para iniciá-lo.

Essa etapa é opcional: se você ignorá-la, o VS Code iniciará o servidor e pedirá confirmação quando o Copilot usar as ferramentas.

## Testar o recurso de filtragem

O Copilot pode abrir páginas, navegar, aplicar filtros, ler os resultados e resumir o que observou. O servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server], em vez de capturas de tela, e o agente trabalha com elementos rotulados, como botões, links e itens de lista.

Com o servidor conectado e o aplicativo em execução, envie:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá o navegador pelo servidor MCP, seguirá as etapas e apresentará os resultados. Compare o relatório aos critérios de aceitação da issue. Se algo estiver incorreto, faça perguntas ou peça que o Copilot corrija o código antes de abrir o PR.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o antes de enviar o prompt. Na primeira utilização, o Copilot pode precisar baixar um navegador; se isso acontecer, peça que execute `npx playwright install chromium`.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, publique a branch `filtering-vscode`.

1. Abra **Source Control** no Codespace e revise as alterações do Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit com o botão **Sparkle**.

   ![Painel Source Control com as alterações do Copilot](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Peça ao Copilot que encontre a issue correspondente e crie um PR associado a ela.

1. No painel Copilot Chat, selecione **New Chat**.
2. Confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Envie:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Selecione **Continue** quando necessário para permitir que o Copilot obtenha informações e execute operações. Na primeira utilização das ferramentas do GitHub, talvez seja necessário autenticar.
5. Observe o Copilot localizar a issue e criar o PR.
6. Abra o link do PR e revise-o, mas **não faça merge ainda**.

## Resumo e próximos passos

Você aprendeu o que é o MCP, adicionou o servidor MCP do Playwright ao espaço de trabalho, usou-o para testar a filtragem no navegador e publicou a branch para abrir um PR.

O MCP é uma maneira de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
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
