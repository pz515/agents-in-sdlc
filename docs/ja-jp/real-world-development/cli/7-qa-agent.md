---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター操作を確認しました。今度は、明確な QA プロセスを定義したカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、指示、スキル、MCP ツールとカスタムエージェントの連携を確認し、再利用可能な品質保証プロファイルを作成して、フィルターの Issue に照らした結果を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫して確認したいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルでは目的、指示、利用可能なツールを指定します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

使用してきたカスタマイズは、それぞれ役割が異なります。リポジトリの指示はチームの標準を示し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザーツールを提供します。QA プロファイルはこれらを使って要件を評価し、結果を報告するよう Copilot に指示します。これらを置き換えたり、別の会話を始めたりするものではありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。QA が行うチェックと守るべき境界の両方を定義します。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロンプトでカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、含めるとエージェントの表示名が明確になります。
3. 要件から始め、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うよう指示されていることを確認します。
4. 根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットしたり PR を作成したりしないことを確認します。
5. 不足や境界の誤りがあれば、続行する前に既定のエージェントに修正を依頼します。

## Issue を基に QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成したら同じフィルター会話を再開し、QA を選択して、Issue と計画の内容をコンテキストとして使います。

1. 次のプロンプトを入力してエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントが選択肢に表示されない場合がありますが、上記のコマンドで有効にできます。

2. 次のプロンプトで機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門的役割を追加し、その動作を確認しました。カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認し、QA プロファイルを作成して、フィルター機能の要件に対する結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
