---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルで、集中した指示、ベストプラクティス、コーディングパターン、分野固有の知識を指定します。チームの専門知識を、[WCAG][wcag] に準拠するアクセシビリティエージェント、安全なコーディング慣行を適用するセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどとして再利用できます。

カスタムエージェントは、プロジェクトの `.github/agents` またはユーザー共通の `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記した Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]はどちらも、AI に作業方法を伝える Markdown ファイルで主に定義します。簡単に言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルや他のエージェントを調整して作業します。このラボでは、アクセシビリティエージェントがサイトをアクセシビリティガイドラインに照らしてレビューし、更新します。その作業の中でプルリクエストワークフローやテスト実行などのスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

この演習では、カスタムエージェントの定義方法を確認し、タスクをエージェントに割り当てます。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディング サイトを使えるように取り組んでいます。現在のダークテーマは、テキストと背景色のコントラストが不十分で読みにくいというフィードバックがありました。デザインチームから、利用者が切り替えられるハイコントラストモードの実装が求められています。カスタムエージェントでこの機能をすばやく追加します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用カスタムエージェントはすでに用意されています。

1. Codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` の YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` の frontmatter が必要です。

3. アクセシブルな Web サイトを生成する際の主な責任、アクセシビリティのベストプラクティス、HTML・CSS・JavaScript のコード例、よくある問題を確認します。

## Issue を作成して割り当てる

エージェントのミッションコントロールでは、cloud agent のタスクを割り当て、監視し、追加のガイダンスを与えられます。ハイコントラストモードの実装を依頼します。

1. リポジトリを開き、**Issues** タブを選びます。
2. **New issue**、続けて **Blank issue** を選択します。
3. **Title** に `Add high contrast mode to website` と入力します。
4. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

5. **Create** を選択します。
6. 右側の **Assign to Copilot** を選択し、一覧から **Accessibility agent** を選びます。

   ![カスタムエージェントとアクセシビリティを選択した cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

7. **Assign** を選択します。Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

カスタムエージェントは、特定のタスクや分野向けの専門的なアシスタントです。チームの専門知識や標準を再利用できるようにし、今回はカスタムエージェントの定義方法を確認して、タスクを割り当てました。

Copilot がハイコントラストモードを実装している間に、次はミッションコントロールで[エージェントのセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを利用する準備][org-custom-agents]
- [Enterprise でカスタムエージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントを監視・管理する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
