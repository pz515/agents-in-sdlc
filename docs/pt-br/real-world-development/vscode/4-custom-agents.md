---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

[Agentes personalizados][custom-agents-concept] permitem criar assistentes de IA especializados em tarefas ou domínios do desenvolvimento. Arquivos Markdown na pasta `.github/agents` fornecem instruções, boas práticas, padrões de código e conhecimento específico para orientar o Copilot. As equipes podem codificar sua experiência em agentes reutilizáveis, como um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança ou um agente de testes.

Os agentes personalizados são definidos por arquivos Markdown em `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de instruções Markdown que definem o comportamento e a especialização do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm semelhanças: ambos usam Markdown para instruir a IA. A distinção mais simples é que o **agente personalizado** executa o trabalho e as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes e poderá usar skills de testes ou pull requests.

> [!NOTE]
> Não há uma única maneira certa de criar um agente. Teste e itere para encontrar o que funciona no seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a pessoas com diferentes capacidades e preferências visuais. Comentários de usuários indicaram que o tema escuro é difícil de ler por causa do contraste insuficiente. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Volte ao Codespace e crie uma branch a partir de `main` para manter o trabalho de acessibilidade separado do PR de filtragem do Exercício 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções sobre responsabilidades e boas práticas de acessibilidade, exemplos de HTML, CSS e JavaScript e erros comuns.

## Usar o agente personalizado no Copilot Chat

O seletor de agentes do Copilot Chat no VS Code lista os agentes definidos em `.github/agents`. Selecione um agente para usar suas instruções e ferramentas.

> [!TIP]
> **Abrir o Copilot Chat**
>
> No Codespace, abra o Copilot Chat e selecione **New Chat**. As etapas indicarão o modo e o modelo quando forem importantes.

1. No seletor de agentes do Chat, escolha **Agent**, se ainda não estiver selecionado.

   ![Seletor de agentes na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra o seletor de agentes na parte inferior do Chat, que exibe o agente ativo, normalmente **default**.
3. Selecione **Accessibility agent**.
4. Envie ao agente:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot explorará o código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá no chat com o caminho e um diff para revisão.
6. Observe o indicador **Files changed**, que mostra os arquivos alterados.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot alterará componentes Astro, estilos e testes relacionados. Você revisará e orientará esse trabalho na próxima lição.

## Resumo e próximos passos

Você explorou [agentes personalizados][custom-agents], assistentes especializados que permitem codificar o conhecimento e os padrões da equipe. Você:

- aprendeu como os agentes personalizados são definidos.
- usou um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que são feitas e solicitando um controle para o modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Exercício anterior: Testar o recurso com o MCP do Playwright][previous-lesson] | [Próximo exercício: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
