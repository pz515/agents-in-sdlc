---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys のコピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、次のレッスンで Copilot CLI のインストールと実行に使う統合ターミナルを備えた [Codespace][codespaces] を起動します。

このレッスンでは、テンプレートから Tailspin Toys プロジェクトのコピーを作成し、Codespace を作成してプロジェクトが準備できたことを確認します。

## ラボ用リポジトリを設定する

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template**、続けて **Create a new repository** を選択して、自分用のリポジトリを作成します。
3. GitHub または Microsoft が主催するイベントでワークショップに参加する場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。
4. 作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこの Issue を使います。自分で作成する必要はありません。

新しいワークショップテンプレートからコピーを作成してください。テンプレートには、リポジトリの指示、アプリケーションコード、テスト、`quality-checks` スキル、バックログが含まれます。古いコピーを使用している場合は、必要なファイルがそろっているか講師に確認してください。

## Codespace を作成する

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードの作成、実行、デバッグができるクラウド開発環境です。

1. 作成したリポジトリを開き、**Code** を選択します。
2. **Codespaces** タブを選び、**Create codespace on main** を選択します。
3. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカルデータベースをインストールします。
4. **Do you trust the authors of the files in this folder?** と表示された場合は **Trust Folder & Continue** を選びます。
5. リポジトリルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

6. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選び、Tailspin Toys が読み込まれることを確認します。
7. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

テンプレートから Tailspin Toys プロジェクトのコピーを作成し、Codespace を準備して、プロジェクトが動作することを確認しました。次は [Codespace に GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
