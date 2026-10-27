---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザー上でフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱い、Copilot で追加の自動化を利用できるようにする必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を拡張し、ツールやサービスを追加できます。

このレッスンでは、Model Context Protocol (MCP) と Copilot CLI での使い方を学び、未設定の場合は Playwright MCP サーバーを追加し、エージェントにブラウザーを操作させてフィルター機能を確認します。

## シナリオ

UI の更新を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように Web サイトを使えるようにし、期待どおりの変更かどうかをより確実に確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは最新の情報にアクセスし、ユーザーに代わって操作できます。

ツールやリソースには、AI エージェントと外部サービスの橋渡しとなる MCP サーバーを通じてアクセスします。各 MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促進するための [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同様に扱ってください。利用前にソースコードを確認し、発行者を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Copilot CLI で Playwright MCP がまだ利用できない場合は、次の手順で追加します。

> [!NOTE]
> すでに Playwright MCP が設定されている場合は、この手順を省略して次に進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類 **STDIO** を確定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を確認するよう Copilot に依頼する

先ほどは機能が期待どおりに動くことを手動で確認しました。今度は Copilot CLI から Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトで、Playwright MCP サーバーを使った検証を依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを指定しなくても Copilot が選択します。利用するサーバーを明示すると、判断を助け、結果を安定させることができます。

2. Copilot がブラウザーを操作して確認する様子を見ます。
3. 結果を読み、すべてが期待どおりに動作することを確かめます。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、作業後にサーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から実際のブラウザーで Playwright MCP サーバーを使って機能を確認しました。MCP と Copilot CLI での利用方法を学び、必要に応じてサーバーを追加し、エージェントにブラウザーを操作させました。

次はスキルとブラウザーツールを組み合わせる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
