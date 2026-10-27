---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para criar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para se integrar ao fluxo de trabalho existente da organização e ajudar a desenvolver as soluções certas da maneira certa. Nesta lição, você começará a seguir um processo típico de desenvolvimento orientado por agentes: partir de uma issue para criar um recurso, verificar o código, validar o comportamento e, por fim, mesclar a alteração ao projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo de trabalho do recurso. Normalmente, você usaria branches ou PRs diferentes para cada tipo de arquivo, mas este workshop usa um atalho para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa com o Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar o recurso no modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar este trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará a validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de escrever código. Use o modo Plan para decidir esses detalhes e, depois, autorize uma implementação com escopo definido no modo Autopilot.

## Contexto

Introduzir agentes de programação com IA ao fluxo de desenvolvimento não muda seus fundamentos. Na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um processo parecido com este:

1. Começar por uma issue registrada que descreva o trabalho.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois que o código for revisado e a integração contínua for aprovada, mesclar a alteração.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos processos segue uma variação desse fluxo.

Seguir essa abordagem ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Começar pela issue

Antes de começar a filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para selecionar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite incluir no contexto uma issue ou um pull request (PR) do GitHub.

## Planejar o recurso de filtragem

O planejamento permite definir a abordagem de implementação antes de delegar a tarefa ao Copilot. Para tarefas complexas, vale a pena reservar um tempo para planejar. Vamos pedir ao Copilot que elabore o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar para o modo Plan. Confirme que o indicador abaixo do prompt exibe **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot explora o projeto e define a melhor abordagem para o plano.

3. O Copilot pode fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Leia o plano.
6. Peça ao Copilot que revise qualquer parte que não corresponda às decisões tomadas.

## Aprovar o Autopilot

Com o plano escrito e revisado, implemente-o usando o Autopilot. Esse modo permite que o Copilot itere até considerar o trabalho concluído.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt exibe **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Revise o código e execute o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não exibe **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas no planejamento.
4. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que resolva eventuais falhas.

## Explorar a nova funcionalidade

Agora que o código parece correto, inicie o aplicativo e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, por editora e as combinações definidas no plano.
4. Confirme que a redefinição dos filtros e os resultados vazios correspondem à issue e às decisões tomadas.
5. Volte ao Codespace e peça ao Copilot que pare o servidor iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma conversa com o Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou o recurso no modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, você verá como as instruções personalizadas orientam o código gerado.

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
