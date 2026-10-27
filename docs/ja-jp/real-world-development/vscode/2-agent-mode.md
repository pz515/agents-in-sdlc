---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

簡単なアプリの変更でも、複数のファイルを更新し、テストなどを実行する必要があります。開発者は通常、関係するファイルを探し、変更し、テストを実行し、問題をデバッグし、変更漏れのファイルを見つけて再度更新します。このような作業を支援するのが Copilot エージェントモードです。

エージェントモードは IDE 内でより自律的に動作します。プロジェクト構成を調べ、必要なファイルを変更し、テストを実行して、見つかった問題を自動的に修正します。エージェントモードで Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> エージェントモードと cloud agent は名前が似ていますが、異なる用途向けです。エージェントモードは IDE で作業し、すばやいフィードバックと対話を可能にします。cloud agent はチームメンバーのように非同期で動き、Issue とプルリクエストを通じて連携します。

この演習では、エージェントモードがプロジェクトを調べ、関連ファイルを見つけ、UI とデータ層にまたがる機能を実装する方法を学びます。さらに、コードベースにマージする前に、Copilot が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャーのヘルパーを追加しました。今回は Copilot エージェントモードで、残りのデータ層、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、現在の Web サイトの機能を確認します。Tailspin Toys はボードゲームのクラウドファンディング サイトです。単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter から Drizzle ORM を通じてローカル SQLite データベースを参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな動作には標準 DOM API を使う小さな Astro `<script>` を使います。

1. Codespace に戻ります。前の演習の後は `main` にいるはずです。
2. 新しいターミナルを開き、フィルター用ブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

3. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

4. ターミナルに表示された `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して Web サイトを開きます。Codespace では `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。

ホームページには、ボードゲームのタイトル、画像、説明が表示されます。ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどが掲載された詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。

テンプレートからプロジェクトを作成すると Issue のバックログも用意されています。Copilot に内容を表示させます。

1. Chat ビューのエージェント選択で **Agent** を選びます。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)
2. 利用可能なモデルから **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> これは特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順に記載しています。LLM の出力は確率的なので、例と異なる提案が表示されることがあります。想定された動作です。

3. 次のプロンプトを送信します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択してすべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧と優先順位を確認します。

## 指示ファイルを確認する

コード生成を始める前に、Copilot にコンテキストを与える `.github/instructions/ui.instructions.md` を確認します。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. アーキテクチャの概要、コンポーネント設計・テスト容易性・アクセシビリティの原則、Astro、Tailwind CSS、Drizzle などの指示へのリンクを確認します。

> [!TIP]
> 指示ファイルから他の指示ファイルやプロジェクト内の例を参照できます。パスは指示ファイルの場所を基準とするため、ガイダンスを再利用し、小さく分割できます。

## フィルター機能を実装する

フィルター機能を完成させるには、データ層 (`src/lib/`) のロジック、動作を確認するテスト、ゲーム一覧ページの UI を更新する必要があります。すべてのテストに成功してからコードをマージします。

1. Copilot との会話を続けるか、**New Chat** で新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. エージェント選択で **Agent**、モデルで **Claude Sonnet 4.5** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)
4. 次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Copilot が関連するデータ層、UI、テストを調べ、変更してテストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テスト実行を行います。完了まで数分かかることがあります。作業中に **Keep** と **Undo** が表示されても、すべての作業が終わるまで選択する必要はありません。

6. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

7. 処理中にテストが失敗することがあります。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで作業します。

   ![Copilot エージェントモードのチャットセッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

8. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot を含む AI ツールが生成したコードは、必ずレビューしてください。

9. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
10. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードで Tailspin Toys に機能を追加しました。エージェントモードで UI とデータ層をまたぐ変更を実装し、プロジェクトを調べて関連ファイルを見つけ、変更とテストをコードベースにマージする前にレビューしました。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### ボーナス演習 — ページ分割を実装する

ゲームの一覧が増えた場合に備えて、ページ分割の追加を Copilot に依頼します。既存の指示ファイルなどのベストプラクティスに従い、ページサイズを利用者が選択するか固定値にするかを決め、希望する動作を明確に伝えます。必要に応じて Copilot に追加の変更やコンテキストを伝えます。

## リソース

- [Copilot の ask、edit、agent モード: 役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
