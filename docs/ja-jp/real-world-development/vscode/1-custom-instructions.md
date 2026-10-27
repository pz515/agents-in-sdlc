---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際はコンテキストが重要です。特定の方法でタスクを実行したい場合や、Copilot に共有すべき背景情報がある場合は、それを参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成の指針を伝えられます。

この演習では、リポジトリのカスタム指示やパス別指示ファイルが、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。フィルター用のデータヘルパーを現在の指示で生成し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加して、同じプロンプトを再実行します。最後に指示とフィルターの一部を `main` にマージし、次の演習で更新後のガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが、指定した標準すべてに従うとは限りません。Copilot の出力は確率的です。この演習では、指示を更新した後の傾向の変化を確認します。出力が完全に同じになることを期待しないでください。

## 指示ファイル

### シナリオ

Tailspin Toys には開発規約があります。データ層には必ずユニットテストを作成し、UI はダークモードでモダンな見た目にします。コードには TSDoc コメントを追加し、各ファイルの先頭にそのファイルの役割を説明するコメントを記述します。指示ファイルを使って、Copilot がこの慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件などのコンテキストを Copilot に提供します。リポジトリ全体、または特定のファイル種別やタスクに指示を適用できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、プロジェクト概要、全体に適用するガイダンスを記載します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストなどのタスクに応じたガイドラインを記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成に利用します。Copilot は AGENTS.md や CLAUDE.md など、他の規約の指示ファイルもサポートしています。

### 指示ファイルを管理するためのベスト プラクティス

- `copilot-instructions.md` にはプロジェクトの概要、構成、全体的なコーディング標準など、プロジェクト全体に関係する情報を記載します。
- `*.instructions.md` にはユニットテスト、Astro コンポーネント、データ層など、特定のファイル種別やタスクのガイダンスを記載します。
- 自然な言葉で、明確なガイダンスと例を示します。

指示ファイルの作成方法は 1 つではありません。実際に試し、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意します。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があります。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境から指示ファイルを生成することもできます。テンプレートを探すには、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示を確認する

1. Codespace に戻り、新しいターミナルを開きます。<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
2. 変更用ブランチを作成します。

   ```bash
   git checkout -b custom-instructions
   ```

3. Codespaces エディターで `.github/copilot-instructions.md` を開き、プロジェクトの説明、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** を確認します。**Code standards** 内の **GitHub Actions Workflows** の指示も確認してください。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` が適用対象の glob を定めていることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テストファイルに適用されます。
6. `.github/instructions/drizzle.instructions.md` の末尾を確認します。他の指示ファイルやプロジェクト内の例へのリンクが、指示を再利用しやすくします。

`.github/copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内のドキュメントはまだ求めていません。次の手順で追加します。

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な選択肢は各手順に記載されています。

1. 以前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント選択で **Agent** を選びます。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパーファイルやテストを更新します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` の規約によるものです。
7. 提案コードには TSDoc コメントとファイル先頭のコメントが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が改善されることを確認するのが目的です。

## リポジトリ全体に適用する標準を追加する

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting requirements** セクションの既存項目の後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. ファイルを保存します。

> [!TIP]
> リポジトリレベルの `.github/copilot-instructions.md` は全体に適用する標準に、`*.instructions.md` は特定の言語やファイル種別、タスクに適用する指示に使います。

## 同じプロンプトを再実行して変化を確認する

1. Copilot Chat で **New Chat** を選び、新しい会話を開始します。
2. `src/lib/publishers.ts` を開きます。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準を Copilot に伝え、次の演習につながる実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

1. 新しいターミナルを開き、次のコマンドで指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

2. 指示、ヘルパー、および Copilot が追加で変更した関連ファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

1. 現在のブランチから `main` へのプルリクエストを作成します。指示の更新と、後続のフィルター作業に向けたパブリッシャーヘルパーの基盤を説明し、関連 Issue を閉じることを明記します。
2. Copilot が GitHub 操作の許可を求めたら、コマンドを確認してから **Allow** を選択します。
3. PR が作成されたら、Copilot に PR をマージし、ローカルのブランチを `main` に戻して最新コードを取得するよう依頼します。
4. GitHub 操作やシェルコマンドの許可を求められたら、内容を確認してから承認します。

## まとめと次のステップ

指示ファイルが Copilot にコンテキストを与える方法を確認し、VS Code の Copilot Chat で、既存指示の下でコードを生成し、リポジトリ全体に適用する標準を追加して、同じプロンプトを再実行しました。指示とヘルパーの基盤を `main` にマージしたので、次の演習で利用できます。

次は [エージェントモード][next-lesson]で、UI、データ層、テストにまたがる機能を追加します。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [より良い Copilot カスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
