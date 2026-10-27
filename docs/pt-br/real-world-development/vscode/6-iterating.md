---
title: "Exercício 6 - Iterar no trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Exercício anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revisar o trabalho

Ao longo deste laboratório, você usou o GitHub Copilot para melhorar a experiência do usuário. Com o modo de agente, adicionou filtragem no cliente e no servidor; com o MCP do Playwright, testou o recurso no navegador; e, com um agente personalizado, implementou controles dos modos de alto contraste e claro. Você também orientou a sessão para ampliar o trabalho. Agora publique as alterações locais e revise-as como a equipe faria.

### Cenário

Os fundamentos do design de software e DevOps não mudam com a IA generativa. Revise o que o Copilot produz e siga o ciclo de revisão habitual. Envie as alterações de acessibilidade do Codespace ao repositório, abra um pull request e revise o diff antes de envolver a equipe.

## Publicar os recursos de acessibilidade

Os controles de alto contraste e modo claro implementados pelo agente personalizado nos [Exercícios 4][exercise-4] e [5][exercise-5] estão no Codespace em commits. Envie-os para uma branch e abra um pull request para revisão da equipe.

1. Volte ao Codespace.
2. Abra **Source Control** no VS Code.
3. Confirme que as alterações de acessibilidade foram registradas em commits. Se houver alterações do Exercício 5 sem commit, prepare-as e faça um commit com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** ou usando **...** → **Push**.
5. Se o VS Code oferecer para abrir a branch no github.com, aceite; caso contrário, acesse o repositório e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro, como `Add high-contrast and light-mode toggles`, e uma descrição breve que explique o que foi feito e por quê.
7. Selecione **Create pull request**.
8. Na guia **Files changed**, revise o trabalho de ponta a ponta. Preste atenção especial a:
   - componentes de interface para alternar os modos.
   - armazenamento local das preferências do usuário.
   - alterações de CSS e estilos para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a troca de modos.
9. Volte à guia **Conversation**.
10. Se algum workflow estiver aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos workflows e confirme que foram aprovados.

> [!TIP]
> Quer uma segunda opinião sobre a acessibilidade? Mencione `@copilot` em um comentário no PR e peça, por exemplo, que procure outros problemas de WCAG ou sugira melhorias de navegação por teclado. O Copilot iniciará uma nova sessão.

## Exercício opcional: continuar explorando localmente

Trabalhar iterativamente com um agente no IDE exige prática. Experimente:

- adicionar um formulário de interesse para apoiadores na página de detalhes de um jogo.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados em `src/lib/`.
- ampliar o escopo do agente de acessibilidade, por exemplo, auditando a ordem de foco do teclado em todo o site.

## Resumo

Parabéns, você concluiu o percurso de VS Code. Você:

- **Usou o MCP do Playwright para testar o recurso manualmente.** Adicionou o servidor e deixou o Copilot controlar um navegador para verificar a filtragem antes do PR.
- **Orientou o modo de agente em alterações coordenadas na stack.** Em uma sessão, adicionou um recurso de filtragem que envolveu cliente, servidor e testes.
- **Usou um agente personalizado.** Selecionou o agente de acessibilidade e acompanhou a implementação do modo de alto contraste.
- **Gerenciou e orientou uma sessão de agente.** Revisou as alterações no editor, aceitou as desejadas e ampliou a sessão para incluir o modo claro.
- **Concluiu o fluxo com um pull request.** Publicou o trabalho local e revisou-o como a equipe faria.

## Revisão e próximos passos

Este é o fim do percurso obrigatório de VS Code. Você pode encerrar o workshop aqui.

Para conhecer recursos de agentes em outros ambientes:

- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com Plan e Autopilot, skills, um agente personalizado de QA, MCP do Playwright, Agent Merge e transições de contexto.
- ☁️ **[Percurso do agente de nuvem](../../cloud/)** — atribua issues ao agente de nuvem, acompanhe sessões na página de agentes e itere de forma assíncrona em pull requests.

Você também pode continuar o trabalho iniciado neste laboratório. O [awesome-copilot][awesome-copilot] oferece arquivos de instruções, agentes personalizados e skills que podem ser adaptados aos seus projetos.

Como extensão opcional, [Incorporar o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar um Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Exercício anterior: Gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
