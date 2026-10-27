---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

[Agentes personalizados][custom-agents-concept] no GitHub Copilot são assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` do repositório fornecem instruções, boas práticas, padrões de código e conhecimento específico para orientar o Copilot. As equipes podem codificar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança ou um agente de testes.

Os agentes personalizados são definidos por arquivos Markdown em `.github/agents` ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Comparação entre agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e ensinam a IA a executar tarefas. A distinção mais simples é: o **agente personalizado** é quem executa o trabalho; as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e até outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar skills para executar testes ou seguir o fluxo de pull requests.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como em outros usos de IA, teste e itere para encontrar o que funciona no seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com agentes personalizados, você explorará:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a pessoas com diferentes capacidades e preferências visuais. Comentários de usuários indicaram que o tema escuro atual é difícil de ler por causa do contraste insuficiente. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para implementar rapidamente essa funcionalidade.

## Revisar o agente personalizado de acessibilidade

O projeto já inclui um agente personalizado de acessibilidade. Volte ao Codespace e revise o arquivo:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que destacam responsabilidades para gerar código acessível, boas práticas, exemplos de HTML, CSS e JavaScript e erros comuns.

## Criar e atribuir uma issue

O painel de agentes do GitHub permite atribuir tarefas ao agente de nuvem, acompanhar o trabalho e fornecer orientação.

1. Acesse o repositório.
2. Selecione a guia **Issues** e, em seguida, **New issue**.
3. Selecione **Blank issue**.
4. Defina **Title** como `Add high contrast mode to website`.
5. Em **Description**, informe:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. Selecione **Create** para criar a issue.
7. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
8. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Atribuição do agente de nuvem com o agente personalizado de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

9. Selecione **Assign**.

O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Você explorou [agentes personalizados][custom-agents], assistentes especializados em tarefas e domínios. Eles permitem codificar os padrões e a experiência da equipe em agentes reutilizáveis. Você:

- aprendeu como os agentes personalizados são definidos.
- atribuiu uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, você pode [acompanhar e orientar a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Exercício anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próximo exercício: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
