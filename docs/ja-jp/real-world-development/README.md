---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、チームで共有するバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントによる作業管理を反復して行います。

## シナリオ

架空の企業 Tailspin Toys で新しく開発者として働きます。Tailspin Toys は開発者向けのテーマを取り入れた、ボードゲームのクラウドファンディング サービスを提供しています。チームのバックログは GitHub Issue として登録済みで、作業を始められます。カテゴリやページネーションによる絞り込みなどの機能に加えて、アクセシビリティやコーディング標準の改善も含まれます。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境から利用できます。自分の開発方法に合った環境を選びます。各ワークショップは独自のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan と Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、便利なスラッシュコマンドを試します。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

非同期エージェントに GitHub Issue を割り当て、作業を指示し、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
