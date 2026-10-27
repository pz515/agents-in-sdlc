---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar recursos. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar as tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel, você pode ver as tarefas com pull requests abertos, explorar as operações realizadas e orientar sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes e acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação do modo de alto contraste, a equipe decidiu incluir também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, faz sentido solicitar esse recurso durante o trabalho. Oriente o agente para que implemente o modo claro junto com o modo de alto contraste.

## Revisar as tarefas do agente de nuvem

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas exibidas nos painéis principal e lateral. Você deve encontrar as tarefas atribuídas, como atualizar a documentação, gerar APIs para modificar jogos e adicionar o modo de alto contraste.
3. Selecione uma tarefa em andamento e revise as operações do Copilot, que podem incluir:
   - verificar o código do repositório.
   - criar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora peça que o Copilot inclua um controle para o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão de modo de alto contraste. O título exato pode variar.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde até que a configuração seja concluída e o agente comece a trabalhar.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

    ![Tarefa do agente de nuvem com a caixa de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot reconhece a solicitação e a incorpora ao trabalho.

## Deixar o Copilot trabalhar

O Copilot incorporará a solicitação ao fluxo de trabalho depois de terminar a etapa em andamento. Isso pode levar alguns minutos; aproveite esse tempo para refletir sobre o que aprendeu.

## Resumo e próximos passos

Você explorou a página de agentes, o painel central para acompanhar e orientar tarefas do agente de nuvem. Você:

- explorou o painel e a página de agentes para monitorar tarefas.
- redirecionou uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho de acessibilidade, prepare-se para [iterar nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Exercício anterior: Agentes personalizados][previous-lesson] | [Próximo exercício: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
