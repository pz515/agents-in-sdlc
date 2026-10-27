---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、プロジェクトとフィルター Issue を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] はターミナルで動作するエージェント型コーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールと連携できます。タスクを任せたり、変更を依頼したりしながら、作業の流れを保てます。まずは使い慣れたツールでインストールします。

このレッスンでは、npm を使って Copilot CLI をインストールし、GitHub アカウントで認証し、ワークショップのリポジトリを信頼して簡単な会話を試し、組み込みの GitHub MCP サーバーでフィルター Issue を見つけます。

## シナリオ

チームは増え続けるバックログに取り組むため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでエージェントを利用できるようにします。このレッスンでインストールと認証を完了し、残りのワークショップに備えます。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js が事前にインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上と表示されるはずです。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証が求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI のアクセスを許可します。
3. Copilot CLI から次の確認が表示されたら、パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

> [!NOTE]
> Codespace では GitHub セッションですでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 基本操作を確認する

通常のシェルプロンプトでは Codespace 上で直接コマンドを実行します。Copilot CLI を起動すると、自然言語はエージェントに送信され、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選び、<kbd>Enter</kbd> を 2 回押して確定します。
2. `/help` で利用可能なコマンドを確認し、<kbd>Esc</kbd> でヘルプ画面を閉じます。
3. 次の質問で動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot がリポジトリを調べてから回答することを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のプロンプトでフィルター Issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き Issue を読みます。最初の変更が終わった後に利用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使い、worktree を自動作成しません。変更を始める前に機能ブランチを作成します。

## ワークショップのショートカットを使う

Copilot CLI は通常、許可済みの範囲外でツールを使う前に確認します。このワークショップでは Codespace 内で承認プロンプトを省き、演習に集中するため `--yolo` を付けて再起動します。

> [!CAUTION]
> `--yolo` は `--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls` を有効にします。Codespace や VM などの分離された環境でのみ使い、日常の開発で既定値としてエイリアスにしないでください。詳細は[ツールの許可と拒否][allow-all-warning]を参照してください。

`--enable-all-github-mcp-tools` は、後続レッスンで Issue やプルリクエストを操作するための GitHub MCP 読み書きツールを有効にします。Codespace はローカルコンピューターへのアクセスを制限しますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリルートから次のコマンドで再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 会話が動作することを確認するため、プロジェクトについて簡単な質問をしてから `/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりしたら、`copilot --resume` で同じ会話とブランチに戻ります。

## まとめと次のステップ

npm で Copilot CLI をインストールして GitHub アカウントで認証し、ワークショップのリポジトリを信頼して簡単な会話を試し、組み込みの GitHub MCP サーバーでフィルター Issue を見つけました。

次は[最初の目的を絞った変更][next-lesson]に取り組み、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
