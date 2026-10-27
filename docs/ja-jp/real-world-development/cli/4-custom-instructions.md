---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、その構成方法も説明できます。フィルターを構築したので、Copilot が参照した指示を確認し、ドキュメントの標準を追加してコードに適用します。

このレッスンでは、リポジトリの指示やパス別の指示ファイルがエージェントにどう届くかを確認し、指示ファイルを更新してコーディング標準を適用し、指示がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発規約があります。

- コメントにはコードの言い換えではなく、意図と明白でない判断を記述する。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明する。該当する場合は、注入可能な `db` 引数も含める。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変わったらコメントも最新に保つ。
- 既存の書式と lint のガイダンスを維持する。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件などのコンテキストを Copilot に与えます。リポジトリ全体に適用する指示と、特定のファイル種別やタスクに適用する指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。多くのチャットや CLI リクエストに関係するプロジェクト情報を記述します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクに応じたガイドラインを記述します。

> [!NOTE]
> その他の指示形式やサポート状況は実行環境によって異なります。形式を利用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示を確認する

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、このリポジトリ内での Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、冒頭の `applyTo` が適用対象の glob を定めていることを確認します。
6. `.github/instructions/drizzle.instructions.md` の他の指示ファイルやプロジェクトファイルへのリンクを確認します。

## チームのガイダンスに合わせて指示を更新する

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを見つけます。
2. 次の項目を最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルター会話のコンテキストを維持したまま、新しい指示を読み込むため会話を再開します。

1. 会話が開いている場合は `/exit` で終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチで次のコマンドを実行します。

   ```bash
   copilot --resume
   ```

   必要に応じてフィルター会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に指示を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルを読み込む方法を確認し、新しい標準を機能に適用しました。次は[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
