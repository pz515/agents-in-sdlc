---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Exercício anterior: Pré-requisitos][previous-lesson] · [Próximo exercício: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Os [arquivos de instruções][instruction-files] fornecem ao Copilot orientações sobre como gerar código e ajudam a comunicar não apenas *o que* você quer, mas também *como* deseja que o trabalho seja feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções do repositório e arquivos com escopo por caminho.
- gerar a primeira parte dos dados de filtragem — um helper de editoras — usando as instruções atuais.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- executar o mesmo prompt novamente e observar o código gerado adotar o novo padrão.
- fazer commit das instruções e da primeira parte da filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos, pois o Copilot é probabilístico. O objetivo é observar a tendência de mudança depois de atualizar as instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento: a camada de dados precisa de testes unitários, a interface deve ter modo escuro e visual moderno, o código deve incluir comentários TSDoc e cada arquivo deve começar com um bloco que explique sua finalidade.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, como convenções de programação, bibliotecas e comentários. Você pode criá-las para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um arquivo enviado ao Copilot em **todas** as solicitações do repositório, com contexto e orientações gerais do projeto.
- `.github/instructions/*.instructions.md`, arquivos com orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para preenchimento de código ou sugestões da próxima edição. O Copilot Chat, o Copilot CLI e o agente de nuvem usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo`. Também há suporte a formatos como `AGENTS.md` e `CLAUDE.md`; consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

### Boas práticas para gerenciar instruções

- Mantenha `copilot-instructions.md` focado em contexto e padrões globais do projeto.
- Use `*.instructions.md` para tipos de arquivo, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras, com exemplos de código correto e incorreto.

Não há uma maneira única de criar instruções ou usar IA. Experimente para descobrir o que funciona no projeto.

> [!TIP]
> Este projeto inclui instruções para [alterações de interface][ui-instructions] e [Astro][astro-instructions]. O Copilot também pode ajudar a gerar arquivos de instruções. No VS Code, use **Configure Chat → Generate Agent Instructions**; no Copilot CLI, use `/init`. Para modelos, explore o [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções deste projeto

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**, inclusive as orientações aninhadas de **GitHub Actions Workflows** em **Code standards**.
3. Explore `.github/instructions`, que contém orientações para Astro, a camada de dados Drizzle, testes e outros temas.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define um glob relativo à raiz do repositório. Por exemplo, arquivos TypeScript que correspondam a `**/*.test.ts` são incluídos.
5. Leia as orientações para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e exemplos do projeto. Esses caminhos são relativos ao próprio arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** documenta os padrões, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará comentários TSDoc e cabeçalhos de arquivo.

## Criar uma branch para as alterações

1. Volte ao Codespace e abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

Envie um prompt com as instruções atuais e, mais tarde, repita-o para comparar o resultado.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat**. A seleção de modo e modelo varia conforme o exercício; as etapas indicarão o que escolher.

1. Feche as guias do editor abertas em exercícios anteriores para limitar o contexto.
2. Abra `src/lib/publishers.ts` para mostrar ao Copilot onde o helper deve ficar.
3. No seletor de agentes do Chat, escolha **Agent** para que o Copilot possa alterar arquivos.

   ![Seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações, possivelmente ao helper e aos testes.
6. Observe que a função tipada recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras, conforme as convenções em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe se faltam comentários TSDoc e um cabeçalho no arquivo.

> [!CAUTION]
> O Copilot pode adicionar comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar o aumento de *consistência* depois da atualização.

## Adicionar um novo padrão ao repositório

1. Reabra `.github/copilot-instructions.md`.
2. Localize **Code formatting requirements**, por volta da linha 27, e observe que ainda não há uma regra de documentação no código.
3. Adicione estas linhas logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Use `.github/copilot-instructions.md` para orientações globais do projeto e `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. A regra de comentários de documentação é válida para todo o projeto.

## Executar novamente o prompt e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat**.
2. Selecione novamente `src/lib/publishers.ts` para fornecer o contexto do arquivo.
3. Envie o mesmo prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto começa com um comentário semelhante a:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

1. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confira as alterações:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Com a branch publicada, crie um pull request associado à issue de padrões de documentação do backlog. O Copilot pode localizar a issue, criar o PR e mesclá-lo com as ferramentas do GitHub conectadas ao projeto.

> [!NOTE]
> Esta é a primeira etapa que pede ao Copilot que interaja com o GitHub. O modelo do projeto já configura as ferramentas; talvez o VS Code solicite autenticação na primeira utilização.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) e confirme que **Agent** está selecionado.
3. Peça ao Copilot que localize a issue e crie um PR a partir da branch atual, descrevendo a atualização das instruções e o helper de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Revise as solicitações de autorização do Copilot antes de selecionar **Allow**.
5. Depois da criação do PR, peça ao Copilot que faça o merge em `main` e atualize a branch local:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Revise as ações do GitHub e os comandos de shell antes de autorizá-los.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot recebe contexto das instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código com as instruções existentes.
- adicionar um padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- repetir o prompt e observar o código proposto adotar o padrão.
- fazer commit das instruções e do helper para que a próxima etapa possa se apoiar neles.

Em seguida, você usará as instruções no [modo de agente][next-lesson] para adicionar um recurso.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
