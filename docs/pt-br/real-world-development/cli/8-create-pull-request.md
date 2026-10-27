---
title: "Lição 8 - Criar e mesclar o PR do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora revise tudo em conjunto e abra um pull request. Você mesclou o PR de avaliação por estrelas manualmente; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, as alterações do recurso, das instruções, da skill e do agente de QA seriam divididas em alguns PRs. Para simplificar o workshop, todo o fluxo de filtragem e qualidade está em uma conversa e uma branch, e será enviado em um único PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de mesclagem.
- inspecionar a alteração completa do recurso e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente depois da revisão e confirmar a mesclagem.

## Cenário

Ao longo do trabalho de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de mesclagem sob o controle de quem desenvolve.

## Sobre o Agent Merge

O **Agent Merge** automatiza o trabalho necessário para integrar um pull request. Quando habilitado, o Copilot resolve o que estiver bloqueando o PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita a mesclagem automática do GitHub quando o repositório permite.

Até aqui, você selecionou **Merge pull request** manualmente. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar o PR

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agentes digitando `/agent`.
3. Selecione **Default** na lista e pressione <kbd>Enter</kbd>.
4. Crie um PR com `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará a integração contínua do PR e fará a mesclagem quando as verificações forem aprovadas.
7. Confirme que o Copilot informa algo semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo e conclua manualmente a mesclagem revisada quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do desenvolvimento, incluindo a geração e validação de código e o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de mesclagem.
- inspecionou a alteração completa do recurso e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente depois da revisão e confirmou a mesclagem.

Em seguida, explore outros [comandos de barra do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
