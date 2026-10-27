---
title: "演習 5 - エージェントを監視・管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムエージェント][previous-lesson] |
|:--|

エージェントモードでは Copilot が自律的にコードベースを調べ、変更を提案し、ファイルを編集してコマンドを実行します。VS Code の Copilot Chat では、そのツール操作やファイル編集をリアルタイムで確認できます。差分をインラインでレビューし、個々の変更を採用または取り消し、追加のプロンプトで作業を調整できます。

この演習では、アクセシビリティエージェントの作業をリアルタイムで監視し、変更をレビューして採用し、進行中のセッションにライトモードを追加するよう指示します。

## シナリオ

[演習 4][previous-lesson]のアクセシビリティエージェントは、ハイコントラストモードを実装中です。作業中にデザインチームから、既定のテーマが暗すぎると感じる利用者のためにライトモードも追加してほしいと依頼がありました。最初から会話をやり直すのではなく、既存のセッションに追加の作業を依頼します。

## Copilot の進捗を監視する

Copilot Chat ビューには、エージェントが調べたファイル、実行したコマンド、提案した編集が順に表示されます。各ファイルの編集差分には **Keep** と **Undo** の操作があります。

1. 演習 4 で使用した Copilot Chat ビューに戻ります。
2. チャットをスクロールし、ツール操作を確認します。ファイルの読み取りや編集、実行を求めるターミナルコマンドが表示されます。
3. 編集差分が表示されたら、**Keep** で採用し、**Undo** で取り消して再度作業させます。

4. チャット上部の **Files changed** を開くと、セッション中にエージェントが変更したファイル全体を確認できます。

> [!NOTE]
> ターミナルコマンドの実行などに許可が必要な場合、エージェントは **Allow** を表示して一時停止します。内容を確認し、許可してもよいコマンドだけ承認してください。

5. ハイコントラストモードの実装が終わり、変更の要約が表示されるまで待ちます。

## ライトモードを追加するようエージェントに指示する

同じチャットセッションを続けると、エージェントは変更済みのコンポーネント、スタイルの方針、local storage の使用方法など、前の作業のコンテキストを保持します。

1. Copilot Chat の入力欄に次のプロンプトを送信します。

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. エージェントは前の変更を確認し、ライトモードの追加を提案します。
3. それぞれの差分を確認し、**Keep** または **Undo** を選びます。
4. トグル UI を意図しない方法でリファクタリングするなど、望ましくない方向に進んだ場合は、チャット入力欄の横にある **Stop** で停止し、追加の指示を送ります。

> [!TIP]
> 進行中のセッションに指示すると、最初からやり直すよりも効率的です。「CSS を簡潔にして、セレクターの重複ではなく CSS 変数を使う」「既存の設定パネルにライトモードを追加し、3 つ目のトグルは作らない」「両方の切り替えにキーボードショートカットも追加する」など、調整や追加を依頼できます。

## 変更全体をレビューする

1. VS Code で **Source Control** ビューを開きます。
2. エージェントが変更したファイル全体を確認します。Astro コンポーネント、スタイル、関連テストなどが含まれます。
3. いくつかの差分を開き、カスタムエージェントのアクセシビリティ指針 (ARIA 属性、キーボード操作、セマンティック HTML、local storage による保存) が反映されていることを確認します。
4. 問題がなければ、Source Control パネルから変更をステージしてコミットします。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat のエージェントモードで、進行中の作業を監視し、個々の編集をレビューして採用または取り消し、追加プロンプトでコンテキストを維持したまま作業を拡張しました。

次の[レッスン][next-lesson]では、アクセシビリティ作業をプルリクエストとして公開し、このラボで作成したカスタムエージェントの変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェントモードを使う][agent-mode]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
