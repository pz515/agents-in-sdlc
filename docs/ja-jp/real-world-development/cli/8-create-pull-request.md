---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージ作業を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキル、QA エージェントを複数の PR に分けます。このワークショップでは作業を簡略化するため、フィルターと品質ワークフローを 1 つの会話とブランチにまとめました。

このレッスンでは、Agent Merge によるマージ工程の自動化について学び、機能全体と検証結果を確認し、フィルター PR を作成して、レビュー後に Agent Merge を有効にします。

## シナリオ

フィルター機能の開発を通じて、Copilot で計画、実装、検証を行いました。Tailspin Toys は残りの PR 作業を自動化しつつ、マージを許可する権限は開発者が保持したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージ可能にするための残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正し、レビューコメントに対応し、必要に応じて rebase するなど、PR を妨げている問題を解決します。その後、リポジトリが許可している場合は GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge を有効にする前に、変更をレビューして問題がないことを確認してください。

## Agent Merge で PR を管理する

1. Codespace に戻ります。
2. `/agent` を入力してエージェント選択ダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを確認し、すべて成功したらマージします。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は理由を確認し、リポジトリで許可されている方法でレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスを自動化しました。Agent Merge の仕組みを学び、変更と検証結果を確認し、フィルター PR を作成して、レビュー後にマージを確認しました。

次は[Copilot CLI の便利なスラッシュコマンド][next-lesson]で、コンテキスト、モデル、共有、任意のクラウド委任を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
