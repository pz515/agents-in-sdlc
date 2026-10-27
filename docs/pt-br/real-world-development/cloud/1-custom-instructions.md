---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica ou houver informações que o Copilot deva conhecer, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] orientam o Copilot sobre *o que* fazer e *como* fazer.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções do repositório e arquivos com escopo por caminho.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, aqui você não executará um prompt antes e depois da alteração. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, portanto o impacto é mais difícil de demonstrar em tempo real. Você verá a influência das instruções quando revisar os pull requests gerados nas próximas lições.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar modo escuro e ter uma aparência moderna.
- O código deve incluir comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Os arquivos de instruções ajudam a garantir que o Copilot siga essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para orientar as sugestões de código. Você pode especificar convenções de programação, bibliotecas e comentários preferidos, com instruções para todo o repositório ou para tipos específicos de arquivos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter contexto no nível do projeto, como a stack, o que está sendo desenvolvido, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md`, arquivos para tarefas ou tipos de arquivo específicos, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para preenchimento de código nem sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem usam instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a outros formatos, como `AGENTS.md` e `CLAUDE.md`. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

- Mantenha `copilot-instructions.md` focado em orientações do projeto, como descrição, estrutura e padrões globais.
- Use `*.instructions.md` para tipos de arquivo ou tarefas específicas, como testes unitários, componentes Astro e camada de dados.
- Use linguagem natural e orientações claras, com exemplos de código correto e incorreto.

Não há uma única maneira certa de criar instruções ou usar IA. Experimente para descobrir o que funciona no projeto.

> [!TIP]
> Um projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Neste repositório há instruções para [alterações de interface][ui-instructions] e [Astro][astro-instructions]. O Copilot também pode ajudar a gerá-las, por exemplo, com **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. Para exemplos, explore o [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções deste projeto

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, examine as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam às interações com o Copilot.
3. Abra `.github/instructions` e explore as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros temas.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, um glob relativo à raiz que determina quais arquivos correspondem às instruções. Por exemplo, ele inclui arquivos de teste TypeScript que correspondam a `**/*.test.ts`.
5. Observe as instruções específicas para testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e exemplos do projeto. Esses links permitem dividir instruções extensas em arquivos menores e reutilizáveis. Os caminhos são relativos ao próprio arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** documenta os padrões de programação, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará comentários TSDoc e cabeçalhos de arquivo.

## Adicionar um novo padrão ao repositório

Este é o único trecho que você editará neste exercício. Adicione uma regra válida para todo o projeto para que a documentação no código inclua comentários TSDoc e um cabeçalho.

1. No terminal do Codespace, crie e selecione uma branch nova:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Reabra `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, por volta da linha 27.
4. Adicione estes itens logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Instruções no nível do repositório são o lugar adequado para padrões globais, como a regra de comentários de documentação. Use arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas.

## Fazer commit, push e merge da atualização de instruções

O agente de nuvem lê as instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, faça merge das instruções em `main` para que o agente as use.

1. Prepare e faça commit da atualização:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a branch e fazer commit diretamente nela. A etapa de branch demonstra um fluxo de trabalho real.

## Resumo e próximos passos

Você explorou como o Copilot usa arquivos de instruções como contexto e adicionou um padrão ao `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente de nuvem gerar.

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
