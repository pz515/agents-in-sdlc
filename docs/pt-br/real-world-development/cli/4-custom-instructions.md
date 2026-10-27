---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *que* código você quer, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções com escopo por caminho chegam ao agente.
- atualizar o arquivo de instruções para que os padrões de programação sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes para as práticas de desenvolvimento:

- Comentários devem explicar a intenção e decisões não óbvias, em vez de repetir o código.
- Funções exportadas em `db/` e `src/lib/` devem documentar finalidade, parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes sobre formatação e lint devem ser preservadas.

Os arquivos de instruções ajudam a garantir que o Copilot tenha as informações necessárias para seguir essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para orientar as sugestões de código. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações relevantes para a maioria das solicitações, como contexto do projeto e boas práticas.
- `.github/instructions/*.instructions.md`, arquivos com orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções do projeto

1. Volte ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Leia a descrição breve do projeto e as orientações de programação, aplicáveis a todas as interações com o Copilot neste repositório.
4. Abra `.github/instructions` e explore os arquivos de instruções para Astro, a camada de dados Drizzle, testes e outros temas.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define o glob dos arquivos aos quais as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos do projeto, que permitem dividir orientações maiores e apontar exemplos ao Copilot.

## Atualizar as instruções para refletir os padrões da equipe

Vamos modificar o arquivo principal `copilot-instructions.md` para incluir uma orientação sobre comentários TSDoc em TypeScript novo.

1. No arquivo `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, por volta da linha 35.
2. Adicione o item a seguir ao final da seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Usar as instruções atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, feche e retome a conversa de filtragem para carregá-las sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit`. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório e na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Observe os comentários TSDoc e confirme que descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI usa arquivos de instruções como contexto e aplicou um novo padrão ao recurso. Você:

- explorou como as instruções do repositório e os arquivos com escopo por caminho chegam ao agente.
- atualizou o arquivo de instruções para reforçar os padrões de programação.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você personalizará e executará a skill reutilizável `quality-checks`.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
