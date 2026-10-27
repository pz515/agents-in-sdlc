---
title: "Lição 5 - Personalizar e usar uma skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desenvolver software envolve mais do que escrever código. Você já validou manualmente o funcionamento do código e usou arquivos de instruções para garantir que ele siga os padrões da equipe. E quanto a testes, lint e outras partes da integração contínua (CI)?

Para esse tipo de tarefa, as **skills de agente** são ideais. Elas ajudam o Copilot a entender como executar operações repetíveis.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

Skills de agente reúnem instruções reutilizáveis, scripts executáveis e recursos de apoio que o agente carrega quando necessário. Em geral, são pastas com um arquivo Markdown chamado `SKILL.md`. O arquivo inclui frontmatter com nome e descrição, uma visão geral da função da skill e orientações sobre quando usá-la. A pasta também pode conter scripts e outros recursos.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas pela equipe ou na pasta pessoal `~/.copilot/skills`.

## Explorar a skill

Explore a skill `quality-checks`, criada pela equipe da Tailspin Toys para executar testes unitários, lint e verificações de tipos.

1. Volte ao Codespace e abra `.github/skills/quality-checks/SKILL.md` no editor do Codespaces.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a identificar quando deve usar a skill.
3. Leia as instruções e observe como orientam o processo de testes e lint.
4. Observe que a skill ainda não tem a seção **Results output formatting**.

## Executar a skill antes de alterá-la

As skills podem ser chamadas diretamente no Copilot CLI ou por linguagem natural. Peça ao Copilot que execute as três verificações:

1. Volte à conversa de filtragem no modo Interactive.
2. Envie este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório apresentado ao final.

## Personalizar o relatório

Vamos atualizar a skill para que o relatório indique o que foi executado, se houve sucesso e o que as ferramentas relataram.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione esta seção ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Envie o mesmo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, permita que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
