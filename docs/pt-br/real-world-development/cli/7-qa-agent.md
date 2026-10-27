---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a filtragem no navegador. Agora, reúna esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado usa instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as descobertas em relação à issue de filtragem.

## Cenário

Antes de abrir um pull request (PR), a Tailspin Toys quer revisar de maneira consistente os requisitos, a qualidade do código, as verificações automatizadas, a cobertura de testes e o comportamento no navegador. Um agente personalizado pode coordenar esse processo e gerar um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown que descreve sua finalidade, suas instruções e as ferramentas disponíveis. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

Cada personalização tem uma função diferente: as instruções do repositório descrevem os padrões da equipe; a skill `quality-checks` reúne verificações repetíveis; o MCP do Playwright fornece ferramentas de navegador; e o perfil de QA orienta o Copilot a avaliar os requisitos e relatar as descobertas. O perfil não substitui esses recursos nem exige outra conversa.

## Criar o perfil de QA

Antes de abrir o PR do recurso, peça ao Copilot que crie um perfil reutilizável de QA, definindo as verificações e os limites que o agente deve seguir.

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecionar o perfil

Revise o perfil antes de usar o agente para confirmar que o fluxo de QA e seus limites estão corretos.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas fornece um nome claro ao agente.
3. Confirme que as instruções mandam o QA começar pelos requisitos, seguir as instruções do repositório, executar a skill `quality-checks` e usar o MCP do Playwright.
4. Confirme que o agente apresenta evidências, pede autorização antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil não incluir essas responsabilidades ou limites, peça ao agente padrão que o revise.

## Executar o QA em relação à issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Retome a conversa de filtragem e selecione o agente de QA para que ele possa usar a issue e as decisões de planejamento já disponíveis no contexto.

1. Ative o agente com este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele ainda não apareça na lista. O comando acima o ativa mesmo assim.

2. Peça ao agente de QA que revise o recurso:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Leia o relatório do agente.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou os resultados. Você:

- explorou como um agente personalizado usa instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as descobertas em relação à issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você reunirá tudo no PR do recurso e usará o Agent Merge.

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
