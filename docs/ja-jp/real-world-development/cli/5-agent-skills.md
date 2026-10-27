---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、手動でコードを確認し、指示ファイルで規約に従わせる必要があります。テストや lint、継続的インテグレーション (CI) のほかの作業も必要です。このようなタスクには **エージェントスキル**が適しています。スキルを使うと、Copilot に適切な操作方法を伝えられます。

このレッスンでは、既存の `quality-checks` スキルを確認し、結果のレポート形式を変更して、更新したスキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルは、エージェントが必要に応じて読み込む再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめます。基本は、スキルの名前を持つフォルダーと、その中の `SKILL.md` です。Markdown には、スキルの名前と説明を定義する frontmatter、概要、呼び出すべきタイミングのガイダンスを記載します。必要に応じて、スクリプトやリソース用のサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルはチームで共有するプロジェクトの `.github/skills` フォルダーか、ユーザー共通の `~/.copilot/skills` フォルダーに置けます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックに使う `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明は Copilot がスキルを呼び出すタイミングを判断する助けになります。
3. テストと lint の手順を確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

1. Interactive モードでフィルターの会話に戻ります。
2. 次のプロンプトを送信して、3 種類のチェックを実行します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行したコマンド、成功したかどうか、ツールが報告した内容を明確に示すレポートを作成します。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルを保存します。

## 更新したスキルを実行する

Copilot CLI は、会話を再起動せずに編集したスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 新しいレポートを読み、最初の結果と比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして実行しました。`quality-checks` スキルでユニットテスト、lint、型チェックを実行し、結果のレポート形式を変更して、スキルを再読み込みしました。この変更はフィルター機能の PR に含まれます。次は [Playwright MCP サーバー][next-lesson]で Copilot がサイトを直接操作できるようにします。

## その他のスキル例

次のコミュニティ例は参考資料であり、追加タスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
