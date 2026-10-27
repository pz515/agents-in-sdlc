---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Exercício anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora o código, propõe alterações, edita arquivos e executa comandos. Como o trabalho acontece na sua máquina em tempo real, o Copilot Chat no VS Code mostra chamadas de ferramenta e edições conforme ocorrem. Você pode revisar diffs, aceitar ou rejeitar alterações e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas.
- orientar a sessão para adicionar um controle do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Durante o trabalho, a equipe de design também solicita um controle do modo claro. Em vez de começar uma conversa nova, oriente a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

O Copilot Chat mostra as etapas do agente — arquivos explorados, comandos executados e edições propostas. Cada edição aparece com um diff e controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, acompanhe as chamadas de ferramenta, leituras e edições de arquivos e os comandos de terminal solicitados.
3. Quando uma edição aparecer:
   - selecione **Keep** para aceitá-la.
   - selecione **Undo** para revertê-la e permitir outra tentativa.
4. Abra **Files changed** no topo do chat para revisar o conjunto de arquivos alterados.

> [!NOTE]
> Se uma ferramenta pedir autorização, por exemplo, para executar um comando de terminal, o agente pausará e exibirá **Allow**. Aprove apenas comandos em que confia.

5. Aguarde o agente concluir o modo de alto contraste e apresentar um resumo.

## Orientar o agente para adicionar o modo claro

Na mesma conversa, solicite um controle do modo claro. O agente manterá o contexto dos componentes, estilos e padrão de armazenamento local usados.

1. Envie este prompt no Copilot Chat:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente examinará as alterações feitas e proporá adições para o modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de modo, interrompa-o com **Stop** e envie uma orientação mais clara.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Peça ajustes, como simplificar o CSS com variáveis, integrar o modo claro ao painel existente ou adicionar atalhos de teclado aos controles.

## Revisar o conjunto de alterações

Antes do commit, revise o trabalho do agente:

1. Abra **Source Control** no VS Code.
2. Revise a lista de arquivos alterados, incluindo componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e confirme o uso de atributos ARIA, navegação por teclado, HTML semântico e armazenamento local.
4. Quando estiver tudo pronto, prepare e faça commit das alterações pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você acompanhou e orientou uma sessão em andamento do Copilot Chat, revisando edições, aceitando ou rejeitando alterações e ampliando o trabalho com prompts de acompanhamento.

Você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou a sessão para ampliar o escopo.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade em um pull request e revise as alterações feitas pelo agente personalizado local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Exercício anterior: Agentes personalizados][previous-lesson] | [Próximo exercício: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
