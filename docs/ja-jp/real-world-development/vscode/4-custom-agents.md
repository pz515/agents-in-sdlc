---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、集中的な指示、ベストプラクティス、コーディングパターン、分野固有の知識を記述します。チームの専門知識を、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディング慣行を適用するセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどとして再利用できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記した Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]はどちらも、AI に作業方法を伝える Markdown ファイルで定義できます。簡単に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを調整して作業します。この演習では、アクセシビリティエージェントがサイトをアクセシビリティガイドラインに照らしてレビューし、更新します。その作業の中で、プルリクエストワークフローやテスト実行などのスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

この演習では、既存のアクセシビリティ用カスタムエージェントを確認し、Copilot Chat で使ってハイコントラストモードを実装します。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディング サイトを使えるように取り組んでいます。現在のダークテーマは、テキストと背景色のコントラストが不十分で読みにくいというフィードバックがありました。デザインチームから、利用者が切り替えられるハイコントラストモードの実装が求められています。アクセシビリティエージェントを使って機能をすばやく追加します。

## アクセシビリティエージェントを確認する

1. Codespace に戻り、`main` からアクセシビリティ作業用のブランチを作成します。フィルター PR とは分けて作業します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` の frontmatter が必要です。

4. アクセシブルな Web サイトを生成する際の主な責任、ベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント選択から、`.github/agents` 内で定義されたカスタムエージェントを選べます。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。

1. Chat ビューのエージェント選択で **Agent** を選びます。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)
2. チャット下部のエージェント選択を開きます。既定では **default** が選択されています。
3. 利用可能なエージェントから **Accessibility agent** を選びます。
4. 次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べて変更を提案し、プロジェクトファイルに編集を適用します。変更ごとにファイルパスと差分が表示されます。
6. **Files changed** の表示で、エージェントが変更したファイルを確認します。

> [!NOTE]
> 実際のファイル変更を伴うため、処理には数分かかる場合があります。Astro コンポーネント、CSS、関連テストなどが更新されます。

次の演習では、進行中の作業を確認し、エージェントに追加の指示を与えます。

## まとめと次のステップ

カスタムエージェントを使うと、特定のタスクや分野に関するチームの専門知識と標準を再利用できます。カスタムエージェントの定義方法を確認し、Copilot Chat のエージェントモードで使用しました。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認し、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントを監視・管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
