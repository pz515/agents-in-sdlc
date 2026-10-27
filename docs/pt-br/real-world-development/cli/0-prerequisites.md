---
title: "Lição 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar as lições do Copilot CLI, você precisa deixar tudo pronto. Você criará sua própria cópia do repositório Tailspin Toys e iniciará um [codespace][codespaces], cujo terminal integrado será usado para instalar e executar o Copilot CLI na próxima lição.

## Configurar o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório modelo][tailspin-template]. O novo repositório contém todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo Tailspin Toys][tailspin-template].
2. Crie sua própria cópia do repositório selecionando **Use this template** e, em seguida, **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver fazendo o workshop como parte de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções fornecidas pelas pessoas mentoras. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Preencha as configurações do repositório criado a partir do modelo](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que você criou (**organization-or-user-name/repository-name**), pois você o usará mais adiante no laboratório.

> [!NOTE]
> **Seu backlog já está pronto**
>
> Quando você cria o repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente. Você trabalhará com essas issues durante todo o workshop; não precisa criar issues manualmente.

## Criar um codespace

Agora, você usará um Codespace para concluir as lições do workshop.

O [GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um editor completo com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório que acabou de criar.
2. Selecione **Code**.
3. Selecione a guia **Codespaces** e, em seguida, **Create codespace on main**.
4. Aguarde a conclusão da configuração do Codespace. O modelo instala as dependências do projeto, o Chromium do Playwright e o banco de dados local.
5. Se aparecer **Do you trust the authors of the files in this folder?**, selecione **Trust Folder & Continue**.
6. Abra um terminal na raiz do repositório e inicie o aplicativo:

   ```bash
   npm run dev
   ```

7. Quando o Codespaces indicar que a porta `4321` está disponível, selecione **Open in Browser** e confirme que o site Tailspin Toys é carregado.
8. Volte ao terminal e pare o servidor de desenvolvimento com <kbd>Ctrl</kbd>+<kbd>C</kbd>.

> [!CAUTION]
> Use uma cópia nova do modelo do workshop. Ela inclui instruções do repositório, o código do aplicativo, testes, a skill `quality-checks` e o backlog necessário. Se usar uma cópia antiga, confirme com a pessoa responsável pelo workshop se ela contém os arquivos necessários.

## Resumo

Você está pronto! Criou uma cópia do projeto Tailspin Toys, iniciou um Codespace e confirmou que o projeto está preparado.

## Próxima etapa

Em seguida, você [instalará o GitHub Copilot CLI][next-lesson] no Codespace e fará a autenticação com sua conta do GitHub.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[tailspin-template]: https://github.com/github-samples/tailspin-toys
[codespaces]: https://github.com/features/codespaces
