---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織が技術的負債を抱えています。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に行います。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せることで、開発者はより大きな課題に集中できます。

この演習では、コード生成環境のカスタマイズ、安全な操作、範囲の明確な Issue の重要性、Issue の Copilot への割り当てを確認します。

## シナリオ

Tailspin Toys では、最初のサイトを作成した開発者がドキュメントを残しておらず、エクスポートされた関数に TSDoc コメントを追加したいと考えています。また、各ゲームの詳細ページに同じカテゴリの関連ゲームを表示し、ゲームを見つけやすくしたいという要望があります。まだ洗練された実装は不要ですが、UX の受け入れテストに使える状態が必要です。どちらも優先度が高い他の Issue によって後回しになりやすいタスクです。cloud agent に非同期で任せると、別の作業を続け、後で結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) はバックグラウンドで作業できます。たとえば [GitHub Issue を Copilot に割り当て][assign-issue]ると、Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境を準備してタスクを始めます。進行中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot を使っても、考える作業を省略できるわけではありません。単純に見える操作にも複雑な部分があります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして、段階を踏み、学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示が Copilot を導く方法を確認しました。次のファイルを GitHub の Web UI または Codespace で読み取り専用で確認し、cloud agent の作業に与える影響を後で見つけられるようにします。

- `.github/copilot-instructions.md`: **Code standards** を確認し、TypeScript の規約と TSDoc コメントに注目します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されるプルリクエストに TSDoc コメント、TypeScript の規約、ファイルヘッダーが反映されているか確認します。

## Copilot cloud agent の開発環境を設定する

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、必要な開発ツールや依存関係を利用できるようになります。このラボには、Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、単一 Astro アプリ用にマイグレーションとシードを行った SQLite データベースを用意する設定が含まれています。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順* に必要な最小限の権限を設定します。Copilot は自身の操作に独自のトークンを使用します。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリを複製する場合は `contents: read` 権限が必要です。複製しない場合は、手順の完了後に Copilot が自動的に複製します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このワークフローには、`copilot-setup-steps` という 1 つのジョブがあります。Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。`workflow_dispatch` トリガーがあるため、Copilot の作業を待たずにリポジトリの **Actions** タブから手動で実行し、正常に動くかテストできます。

## ドキュメントを追加するタスク

古い情報やドキュメント不足は、開発を遅らせ、コードベースの保守や新しい開発者の参加を難しくします。ドキュメントの追加は cloud agent に適した作業です。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブ、続けて **New issue** を選択します。
3. **Blank issue** を選択します。
4. **Title** に `Code lacks documentation` と入力します。
5. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

6. **Create** を選択します。
7. 右側の **Assign to Copilot** を選択し、ダイアログで **Assign** を選択します。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

8. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

9. **Pull Requests** タブから、`[WIP]: Code lacks documentation` のようなタイトルのプルリクエストを開きます。表示されない場合は少し待ち、ページを更新します。
10. 数分後、Copilot の TODO リストを確認します。セットアップ、プロジェクト分析、作業計画に時間がかかる場合があります。
11. プルリクエストのタイムラインで Copilot が Issue に着手したことを確認し、**View session** を選択します。

    ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ページを再読み込みする必要がある場合があります。

セッションでは、Copilot がコードを調査し、状況を把握して計画を立て、コードを生成する過程を確認できます。処理には数分かかる場合があります。非同期作業の間に、次のタスクを割り当てます。

## ゲーム詳細ページに関連ゲームを追加する

1. リポジトリで **Issues**、続けて **New issue** を選び、**Blank issue** を選択します。
2. **Title** に `Show related games on the game details page` と入力します。
3. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

4. **Create** を選択し、右側の **Assign to Copilot** を選んでから **Assign** を選択します。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

まもなく、Copilot が作業を始めたことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

cloud agent は非同期に作業するため、完了後に結果をレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] に Issue を割り当て、非同期に作業させる方法を学びました。技術的負債の解消、新しい機能の作成、フレームワーク間のコード移行などに活用できます。開発環境のカスタマイズ、安全な操作、明確な Issue の重要性、Issue の割り当てを確認しました。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も適用されます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
