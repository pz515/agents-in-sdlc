---
title: "Exercício 2 - Adicionar recursos com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até uma alteração simples em um aplicativo pode envolver vários arquivos, testes e depuração. O modo de agente do Copilot pode explorar a estrutura do projeto, identificar os arquivos relevantes, coordenar alterações, executar tarefas e corrigir problemas.

> [!NOTE]
> O modo de agente e o agente de nuvem são experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, usando issues e pull requests.

Neste exercício, você vai:

- usar o modo de agente para explorar o projeto e coordenar alterações em arquivos.
- implementar um recurso nas camadas de interface e dados.
- revisar alterações e testes antes do merge.

## Cenário

À medida que a lista de jogos cresce, permita filtrar por categoria e editora. Você criou um helper de editoras no exercício anterior; agora concluirá as alterações restantes da camada de dados, da interface e dos testes com o modo de agente.

## Executar o site Tailspin Toys

O site é uma aplicação Astro que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local com Drizzle ORM. Não há uma API de backend separada nem um framework de interface no cliente. Helpers reutilizáveis de acesso a dados ficam em `src/lib/`; a interatividade usa scripts Astro pequenos e APIs DOM padrão.

1. Volte ao Codespace. Depois do exercício anterior, você deve estar em `main`.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, o terminal mostrará um endereço como `http://localhost:4321`.

5. Se o VS Code perguntar se deve abrir o navegador, feche essa caixa de diálogo. Abra o endereço do servidor no terminal mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado ao selecioná-lo. No Codespace, o link local será encaminhado para uma URL privada do Codespaces.

Explore a página inicial, que lista os jogos, e as páginas de detalhes, que mostram informações como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> No Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. As etapas indicarão o modo e o modelo quando forem importantes.

1. No seletor de agentes do Chat, escolha **Agent**.
2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> A menção ao Claude Sonnet 4.5 não expressa preferência por um modelo. Ele foi usado para criar este workshop e é indicado para ajudar a manter uma experiência consistente. Como os LLMs são probabilísticos, as sugestões podem variar; isso é normal.

> [!NOTE]
> O Copilot pode usar um comando MCP diferente, mas ainda deve conseguir concluir a tarefa.

3. Envie este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o comando liste as issues.
5. Revise a lista e a priorização proposta.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, leia `.github/instructions/ui.instructions.md`, que fornece contexto sobre como adicionar recursos ao site.

1. Abra `.github/instructions/ui.instructions.md`.
2. Observe a arquitetura, os princípios de design, testabilidade e acessibilidade, além dos links para instruções de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Arquivos de instruções podem referenciar outros arquivos e exemplos do projeto. Os caminhos são relativos ao arquivo de instruções, o que facilita reutilizar conteúdo e dividir orientações complexas.

## Implementar a filtragem

Para concluir a filtragem, o aplicativo precisa de alterações na camada de dados (`src/lib/`), testes do comportamento e uma interface de filtragem na página de listagem de jogos.

1. Continue na conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e o arquivo `ui`.

   ![Exemplo de seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** está selecionado.
4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Envie este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto, localizar helpers, interface e testes, alterar os arquivos e executar os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot alterna entre geração de código e testes e pode mostrar os controles **Keep** e **Undo**. Não é necessário selecioná-los enquanto o trabalho está em andamento.

7. Quando solicitado, selecione **Continue** para executar os testes.
8. Alguns testes podem falhar durante o processo. O Copilot itera até concluir a tarefa e não detectar erros.
9. Revise o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador e explore o recurso.
11. Quando tiver confirmado o funcionamento e revisado o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente para adicionar recursos ao site Tailspin Toys e aprendeu a:

- implementar recursos nas camadas de interface e dados.
- explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes antes do merge.

Em seguida, [teste o recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra: implementar paginação

À medida que a lista de jogos crescer, poderá ser necessário adicionar paginação. Use as habilidades deste exercício para pedir ao Copilot que implemente o recurso. Considere se o usuário poderá escolher o tamanho da página, use os arquivos de instruções existentes e forneça contexto suficiente. Talvez seja necessário iterar e pedir ajustes; isso faz parte do trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
