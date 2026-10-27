---
title: "演習 4 - エージェントを監視・管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の演習では、GitHub Copilot cloud agent にユーザー体験の改善や機能追加など、3 つのタスクを割り当てました。cloud agent は非同期かつ自律的に作業しますが、進行中のタスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。ここでは、オープンなプルリクエスト (PR) があるエージェントタスクを確認し、実行した操作を調べ、進行中のセッションに追加の指示を与えられます。

このレッスンでは、エージェントページを使って cloud agent のタスクを監視し、進行中のセッションに追加機能を依頼します。

## シナリオ

チームはハイコントラストモードに加えてライトモードも必要だと気付きました。スタイルと切り替え機能をすでに更新中なので、エージェントに作業を追加し、両方のモードを実装させます。

## Copilot cloud agent のタスクを確認する

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストから、割り当てたタスクを確認します。コードのドキュメント更新、ゲームを変更する API の生成、サイトのハイコントラストモード追加などが表示されます。
3. 実行中のタスクを選び、Copilot が行った操作を確認します。リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、タスクを完了するための操作などが含まれます。

> [!NOTE]
> 表示される手順は Copilot の作業状況や進め方によって異なります。

4. 右側のプルリクエストペインで PR と変更ファイルも確認できます。

## Cloud agent に追加の指示を与える

1. ハイコントラストモードのセッションを選択します。タイトルは Copilot の命名や作業状況によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで cloud agent の作業中にセッションへ指示するダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に加えたことを確認します。

## Copilot に作業を続けさせる

Copilot は、メッセージを送信した時点で作業していたステップを終えた後、追加依頼を取り込んで作業を続けます。完了までには数分かかる場合があります。その間に、学んだ内容を振り返ってもよいでしょう。

## まとめと次のステップ

Copilot agents ページを使って、cloud agent の実行中・完了済みタスクを追跡し、作業をレビューし、進行中のタスクに追加のガイダンスを与えました。アクセシビリティ機能の作業が進む間に、Copilot が作成した PR を[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
