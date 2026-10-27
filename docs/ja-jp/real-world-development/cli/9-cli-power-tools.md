---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、多くの CLI ツールと同様に、操作に使えるスラッシュコマンドが用意されています。高度な機能、内部情報、追加の構成オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、その他の便利なコマンドを確認します。

このレッスンでは、`/context` と `/compact` で会話コンテキストを調べ、`/model` で利用可能なモデルを確認し、`/share` でセッションをエクスポートまたは共有する方法を学びます。さらに、並列作業、worktree、クラウドエージェントへの委任に関する任意のコマンドを確認します。

## シナリオ

CLI の主なワークフローが完了しました。ここでは、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドで状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域へのコンテキスト配分を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的に圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。関係のない別タスクに移るときは、`/clear` または `/new` で新しい会話を始めます。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも違います。Copilot CLI では、利用可能なモデルを一覧表示して選べます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学びを共有すると、AI ツールの活用を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. 共有する場合は、ローカルの Markdown にエクスポートする `/share file` など、目的に合った方法を選びます。
3. エクスポート内容を他の人に送ったり公開したりする前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の拡張や委任

ワークショップの主要な内容は完了です。Copilot CLI には、大規模なタスクに役立つ次のコマンドもあります。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分け、並列実行できます。
- `/worktree` は、別タスク用の独立した Git worktree を作成できます。
- `/delegate` は、非同期で作業してプルリクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作成するため、任意の操作です。試す場合は、範囲の明確な新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶには、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

スラッシュコマンドを使うと、Copilot CLI を構成し、セッションを共有し、内部の動作を確認できます。コンテキストの確認と圧縮、モデル選択、セッション共有、並列作業やクラウドエージェントへの任意の委任を学びました。

ほかにも多くのスラッシュコマンドがあります。次は[学んだ内容を振り返り][next-lesson]、さらに学ぶための次のステップを確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
