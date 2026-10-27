---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot CLI][about-copilot-cli]** は、ターミナルで GitHub Copilot をエージェント型のコーディングアシスタントとして利用できるようにします。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンドラインから行えるため、グラフィカルエディターに切り替えずに作業の流れを保てます。

このワークショップでは、Copilot CLI のインストールから始め、小さな星評価の変更を加えて最初のプルリクエストをマージします。続いて Plan モードでフィルターの要件を決め、Autopilot モードで実装し、Interactive モードでレビューします。リポジトリの指示を更新し、`quality-checks` スキルを調整し、Playwright MCP を使ってブラウザーで動作を確認します。QA カスタムエージェントで要件、テスト、検証結果をまとめ、Agent Merge でフィルター PR を処理します。最後に、コンテキスト管理、モデル選択、セッション共有、任意のクラウド委任に使うスラッシュコマンドを確認します。[Foundry を使うオプションの 3 モジュール シリーズ][foundry]では、モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続できます。

ワークショップでは、星評価の変更と、指示・スキル・QA プロファイル・テストを含むフィルター機能の 2 つの PR を作成します。フィルターと品質ワークフローは、会話とブランチを共有します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証する |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して Interactive モードでレビューする |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認・更新し、フィルター機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存スキルのレポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザー確認 | CLI で MCP を設定し、ブラウザーでフィルターを確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門プロファイルを作成して選び、最終検証の根拠を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、クラウドへの任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、参考資料を振り返る |
| [任意: Foundry を取り入れる][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイしてサイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] ターミナル / コマンド ライン操作の基本的な知識
- [ ] インストール済みで設定済みの Git

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、code review、Copilot CLI の機能が含まれているため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
