---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を構築しました。プルリクエストを作成する前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに **Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストし、ブランチを公開して PR を作成します。

この演習では、Model Context Protocol (MCP) と MCP サーバーによる Copilot の機能拡張を学び、Playwright MCP サーバーをワークスペースに追加し、Copilot にブラウザー上で機能をテストさせ、フィルター作業のブランチを公開してプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外にあるツールを利用すると、さらに便利になります。Model Context Protocol (MCP) は、エージェントが外部のツールやサービスと通信するための標準です。

![エージェントモードがコンテキスト、LLM、ツールと連携する仕組みを示す図。MCP サーバーと VS Code 拡張機能が提供するツールを含む](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスとリアルタイムで通信できます。リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

ツールやリソースは、AI エージェントと外部サービスの間を仲介する MCP サーバーから利用します。サーバーは既存 API や npm パッケージなどのローカルツールとの通信を管理し、エージェントが使えるツールやリソースを提供します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright でブラウザーを自動操作し、ページ移動、フォーム入力、ボタン選択などを行えます。

GitHub は、MCP サーバーを見つけやすくし、エコシステムへの貢献を促進するための [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーは他の依存関係と同様に扱ってください。利用前にソースコードを確認し、発行者を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` で、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに設定されている `github` サーバーを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` エントリはプロジェクトテンプレートに含まれています。前の演習で Copilot がバックログの Issue を読めたのはこのためです。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使い、HTTP 経由で VS Code が接続します。Copilot が初めてツールを使う際に GitHub で認証します。

## Playwright MCP サーバーを追加する

[Playwright MCP サーバー][playwright-mcp-server]を追加すると、Copilot が操作できるブラウザーを使って機能をテストできます。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカルサーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面を表示せずにブラウザーを実行する指定です。デスクトップ環境のない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys では E2E テストに Playwright を使っているため、必要なブラウザーは通常すでにインストールされています。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。初めてツールが必要になったときにサーバーが起動し、信頼するか確認されます。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** を選択し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認して信頼します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて使うときに VS Code が起動し、信頼するか尋ねます。

## フィルター機能をテストする

Playwright MCP サーバーを使うと、Copilot がブラウザーを開き、ページを移動してフィルターを適用し、結果を読み取って報告できます。ページの[アクセシビリティ ツリー][playwright-mcp-server]を使って、ラベル付きのボタン、リンク、リスト項目などの構造化された要素を認識します。そのため、機能確認とともに簡単なアクセシビリティ確認もできます。

サーバーを接続しアプリを起動した状態で、Copilot に次の操作を依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーでブラウザーを起動し、各手順を確認して結果を報告します。Issue の受け入れ条件と照らし合わせます。問題があれば、PR を作成する前に質問するか、コードの修正を依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP サーバーを初めて使うときにブラウザーがダウンロードされる場合があります。不足していると報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンでコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

PR を作成する方法はいくつかありますが、GitHub ツールを使える Copilot に任せます。

1. Copilot Chat パネルに移動し、**New Chat** で新しい会話を始めます。
2. エージェント選択で **Agent** を選びます。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選び、Copilot が情報を集めて操作を行うことを許可します。GitHub ツールを初めて使うときは、GitHub への認証が求められることがあります。
5. Copilot が Issue を検索し、適切な PR を作成する様子を確認します。
6. Copilot が示したリンクから PR を確認します。**まだマージしないでください**。

## まとめと次のステップ

MCP と MCP サーバーによる Copilot の機能拡張を学び、Playwright MCP サーバーをワークスペースに追加し、ブラウザーでフィルター機能をテストしました。ブランチを公開し、フィルター作業のプルリクエストも作成しました。

MCP サーバーで機能をテストしました。次はアクセシビリティレビューなど、目的を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
