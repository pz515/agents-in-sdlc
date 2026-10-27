---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が使えることを確認します。

## ラボ用リポジトリを設定する

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボのページで **Use this template** を選択し、**Create a new repository** を選択してリポジトリのコピーを作成します。

    ![このテンプレートを使用ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加する場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

    ![リポジトリテンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。

> [!NOTE]
> **バックログの準備**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこの Issue を使います。自分で作成する必要はありません。

## Codespace を作成する

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手作業でインストールするよりも早く完了します。その間に GitHub Copilot の機能を確認してもよいでしょう。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。今はブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作します。どちらも必要な前提条件が整った環境を提供します。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使う

VS Code で GitHub Copilot Chat のエージェントモードを使うには、IDE に GitHub Copilot Chat 拡張機能が必要です。GitHub Codespace を使っている場合は、通常すでにインストールされています。

> [!TIP]
> 拡張機能がない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]するか、Visual Studio Code の拡張機能ビューで **GitHub Copilot Chat** を検索し、**Install** を選択します。

インストール後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、Codespace 上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウで「Hello world」のようなメッセージを入力し、Enter キーを押します。
4. 認証されていない場合は、GitHub アカウントへのサインインが求められます。画面の指示に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証すると Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ用リポジトリのコピーを作成し、コードを書くための Codespace を準備しました。

## 次のステップ

[演習 1 - カスタム指示][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
