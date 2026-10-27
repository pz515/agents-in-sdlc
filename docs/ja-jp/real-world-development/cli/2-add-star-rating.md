---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲームカードを変更し、転送ブラウザーで確認して、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えましょう。小さな変更から始めます。ゲームデータには星評価がすでにありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに評価を表示させ、変更をレビューし、最初のプルリクエストとしてマージします。

このレッスンでは、次の内容を学習します。

- 機能ブランチで目的を絞った Copilot の会話を始める。
- エージェントに小さな変更を依頼する。
- `/diff` で変更をレビューする。
- 転送ブラウザーでアプリを実行して変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードにはタイトル、カテゴリ、パブリッシャー、説明しか表示されません。準備運動として、既存の評価を各カードに表示します。最初のセッションに適した、小さく独立した変更です。

## 会話の構造

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話では専用の worktree を作成せず、ターミナルで現在チェックアウトされているリポジトリと Git ブランチを使います。保存した会話に後から戻ることができますが、ファイルとブランチはディスク上の通常の Git 状態として残ります。

会話では、プロンプトとエージェントの応答、ファイルを探索・編集するエージェントのツール操作、`/diff` で確認できる変更の 3 つを確認できます。

## 会話を始めて変更を依頼する

1. Codespace に戻ります。
2. 以前のターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲーム詳細を表示するファイルを見つけて必要なコードを作成します。Copilot CLI で最初の機能を追加できました。

## 差分をレビューする

AI が生成した変更は、小さなものでもマージ前にレビューします。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. 評価がある場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストが両方の状態をカバーしていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot は、すべての生成 AI ツールと同様に確率的に動作するため、生成されるコードは異なる場合があります。完全に同じ実装を期待するのではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると決めつけず、Copilot に Web サイトを起動させ、Codespaces で転送されたブラウザーから更新された UI を確認します。

1. 次のプロンプトでアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、次のプロンプトで起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

1. 既定のエージェントに、次のプロンプトで変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は既存のコミットをプッシュしてプルリクエストを作成し、URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して開きます。
4. 変更されたファイルとチェックをレビューします。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って最初の変更をリリースしました。機能ブランチで会話を始め、ゲームカードへの変更を依頼し、`/diff` でレビューし、転送ブラウザーで動作を確かめ、最初のプルリクエストを作成してマージしました。

次は[フィルターの Issue から始め、Plan と Autopilot モードを使って][next-lesson]より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
