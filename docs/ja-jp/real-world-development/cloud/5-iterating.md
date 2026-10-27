---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加に関する複数のタスクを GitHub Copilot に依頼しました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能、ハイコントラストとライトモードを含むアクセシビリティ機能を実装しました。コード変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスを進めます。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する提案を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督なしでタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みはコード用ブランチに限られます。
- Cloud agent は GitHub Actions 内に一時的な独立環境を作成して実行されます。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは既定で制限][agent-firewall]されています。

## 生成されたドキュメントをレビューする

まず、Copilot cloud agent が作成した最初のプルリクエスト (PR) である、コードへのドキュメント追加を確認します。

> [!NOTE]
> PR を確認する際に、GitHub Copilot がファイアウォールでブロックされているという警告が表示されることがあります。外部リソース (外部 MCP サーバーを含む) へのアクセスは既定で制限されているため、これは想定された動作です。必要に応じて [cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻り、**Pull Requests** タブを選びます。
2. **Add missing documentation** などのタイトルの PR を開きます。Copilot が作業中ならタイトルに **[WIP]** が付いているので、完了まで待ちます。
3. **Files changed** タブを選び、変更をレビューします。

   ![変更されたファイルのタブ](../../../_images/shared-pr-files-changed.png)

4. 追加された TSDoc コメントやその他のドキュメントを確認します。具体的な変更内容は異なります。演習 2 で確認した指示ファイルに基づく TSDoc コメント、TypeScript 規約、ファイルヘッダーに注目します。
5. 問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちになっている場合は、**Approve and run workflows** を選択します。

   ![ワークフローの承認と実行](../../../_images/shared-approve-workflows.png)

7. PR のチェックでワークフローが実行され、単一 Astro アプリのプロジェクトチェックが成功することを確認します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR やコードへのコメントで **@copilot** をメンションすると、Copilot に変更を依頼できます。Copilot はコメントを確認し、対応する別のセッションを開始します。結果は確率的なため、必ず同じ指示になるわけではありません。たとえば、次のような依頼ができます。

- 各コードファイルの先頭に、役割を説明するコメントを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. **@copilot** をメンションし、生成されたドキュメントの変更を依頼するコメントを追加します。
2. **View Session** を選択し、Copilot が変更する様子を確認します。更新用の新しいセッションが始まります。
3. **Back to pull request** を選択して PR に戻ります。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、新しいコミットが PR に追加されています。
5. **Files changed** タブで変更をレビューします。必要に応じて作業を反復し、完了したらドラフト PR をレビュー可能な状態にして `main` にマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

1. リポジトリの **Pull Requests** タブを開きます。
2. **Show related games on the game details page** などのタイトルの PR を選びます。
3. **Files changed** タブで生成コードをレビューします。
4. 問題がなければ **Conversation** タブに戻り、ワークフローが承認待ちになっているか確認します。必要に応じて **Approve and run workflows** を選び、単一 Astro アプリのチェックが成功するまで待ちます。
5. **任意:** Copilot が作成したブランチに切り替えて機能を手動テストできます。`<branch-name>` を実際のブランチ名 (例: **copilot/fix-8**) に置き換えます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

関連ゲーム機能が完成したら、cloud agent に変更を追加で依頼できます。たとえば表示する関連ゲームの数を調整したり、カスタム指示を更新する前に割り当てたタスクに対してコメントヘッダーと TSDoc コメントを加えたりできます。**Conversation** タブにコメントを追加すると、Copilot はその内容を見て新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、カスタムアクセシビリティエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で追加を依頼したライトモードの両方が含まれているはずです。

1. リポジトリの **Pull Requests** タブを開きます。
2. **Add high contrast mode to website** などのタイトルの PR を選びます。Copilot が作業中なら **[WIP]** の表示が消えるまで待ちます。
3. **Files changed** タブで生成されたコードをレビューします。モード切り替え UI、local storage による設定の保存、ハイコントラストとライトモードのスタイル、ARIA 属性やキーボード操作、モード切り替えの JavaScript/TypeScript に注目します。
4. 問題がなければ **Conversation** タブに戻り、ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。単一 Astro アプリのチェックが成功するまで確認します。
5. **任意:** Copilot が作成したブランチに切り替えて、機能を手動テストできます。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリを起動し、ハイコントラストとライトモードの切り替えが動作し、ページを再読み込みしても設定が保存されていることを確認します。

カスタムアクセシビリティエージェントがアクセシビリティのベストプラクティスに沿った実装を促したことを確認します。問題や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent に実際のバックログ作業を任せると、その間に別の作業を進められます。リポジトリに追加の Issue を作り、Copilot に割り当ててみましょう。

- ゲーム詳細ページに支援者向けの興味登録フォームを作る。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent コースを完了しました。カスタム指示ファイルを確認し、Copilot cloud agent に Issue を割り当て、非同期の計画と実装を確認しました。アクセシビリティ用のカスタムエージェントでハイコントラストとライトモードを追加し、Copilot agents ページからセッションを監視して方向付けました。最後に、Copilot の PR をレビューし、コメントで更新を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent コースを完了しました。さらに学ぶ場合は、次のコースも確認してください。

- 🖥️ **[VS Code コース](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI コース](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA エージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルからレビュー済みの変更を届けます。

自分のリポジトリでは、リファクタリングやテストカバレッジの Issue を cloud agent に割り当てたり、別分野のカスタムエージェントを作成したり、別の技術スタック向けに `copilot-setup-steps.yml` を設定したりできます。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
