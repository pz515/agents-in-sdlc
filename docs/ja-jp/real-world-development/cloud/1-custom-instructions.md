---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に共有すべき背景情報がある場合は、それを参照できるようにします。[指示ファイル][instruction-files]を使うと、何を実行するかだけでなく、どのように実行するかも Copilot に伝えられます。

この演習では、リポジトリのカスタム指示やパス別指示ファイルがプロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。

> [!NOTE]
> VS Code や CLI の演習とは異なり、ここでは指示を追加する前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue で非同期に動作するため、リアルタイムの比較が難しいためです。後の演習で cloud agent が作成したプルリクエストをレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には開発規約があります。データ層には必ずユニットテストを作成し、UI はダークモードでモダンな見た目にします。コードには TSDoc コメントを追加し、各ファイルの先頭に役割を説明するコメントを記述します。指示ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件などのコンテキストを Copilot に提供します。リポジトリ全体、または特定のファイル種別やタスクに指示を適用できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、プロジェクト概要、ベストプラクティスなど、全体に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストなど、特定のタスクやファイル種別にガイダンスを提供します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成に利用します。Copilot は AGENTS.md や CLAUDE.md など、他の標準の指示ファイルもサポートしています。

### 指示ファイルを管理するためのベスト プラクティス

- `copilot-instructions.md` にはプロジェクトの説明、構成、全体的なコーディング標準など、プロジェクト全体に関係する情報を記載します。
- `*.instructions.md` にはユニットテスト、Astro コンポーネント、データ層など、特定のファイル種別やタスクのガイダンスを記載します。
- 自然な言葉で、明確なガイダンスと例を示します。

指示ファイルの作成方法は 1 つではありません。実際に試し、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意します。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があります。Copilot は指示ファイルの生成も支援できます。各環境で方法は異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。テンプレートを探すには、指示ファイルやカスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** 内の **GitHub Actions Workflows** の指示は、Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、冒頭の `applyTo` がリポジトリルートを基準とする glob を定めていることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルに適用されます。
5. ユニットテストの作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾で `unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクを確認します。これにより大きな指示を再利用しやすい小さなファイルに分け、Copilot に例を示せます。ここに記載されたパスはリポジトリルートではなく指示ファイルを基準にしています。

`.github/copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で追加します。

## リポジトリ全体に適用する標準を追加する

cloud agent は Issue が対象とするブランチから指示ファイルを読み取ります。次の演習で Issue を割り当てると cloud agent は `main` からブランチを作るため、指示の変更を `main` にマージしておく必要があります。

1. Codespace のターミナルで作業ブランチを作成します。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き、**Code formatting requirements** セクションを見つけます。ここには既存の規約がありますが、コード内のドキュメントに関するルールはありません。
3. 次の項目を既存の規約の下に追加し、ファイルを保存します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

> [!TIP]
> リポジトリレベルの `.github/copilot-instructions.md` は全体に適用する標準に、`*.instructions.md` は特定の言語、ファイル種別、タスクに適用するガイダンスに使います。

## 指示の更新をコミット、プッシュ、マージする

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue を割り当てる際、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` にコミットしてもかまいません。ここでは実際のプロジェクトに近い手順を示しています。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りの演習で cloud agent が作成するプルリクエストから、標準が反映されているかを確認します。

次は[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [より良い Copilot カスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
