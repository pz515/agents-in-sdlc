---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。Codespace を準備して Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター Issue を見つけました。星評価を追加し、転送ブラウザーで確認して最初のプルリクエスト (PR) を手動でマージしました。フィルター Issue から Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。

さらに、カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。Playwright Model Context Protocol (MCP) サーバーを追加して実際のブラウザーでフィルターを確認し、品質保証 (QA) カスタムエージェントで要件、カバレッジ、スキルの結果、ブラウザーの根拠を評価しました。最後にフィルターの変更全体をレビューし、Agent Merge を許可しました。また、コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドも確認しました。

## リリースした内容

このワークショップでは、更新済みの `main` からそれぞれブランチを作成し、2 つの PR を作ります。

1. **星評価:** ゲームカードに既存の `starRating` と未評価状態を表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを追加します。

フィルターの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー確認を実施しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、指定した形式で結果を報告します。QA エージェントは、要件とテストカバレッジのレビューにその結果を加えてまとめます。

追加するテストは実際の不足を埋めるものである必要があります。QA の結果として新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと根拠を確認し、変更後は影響を受ける検証結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらを会話間で再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の検討、**Interactive** は重点を絞った変更を一緒に進める場合、**Autopilot** は範囲が明確なタスクに適しています。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えることで、出力は大きく変わります。

## さらに試す

主要なワークフローは完了しました。次の CLI 機能も試せます。

- `/review` でコードレビューエージェントに変更を分析させる。
- `/rubber-duck` で問題を説明し、別の視点を得る。
- `/fleet` で独立したサブタスクを並列に調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

どのツールも、使い続けることで上達します。実務や趣味のコード、作りたいと思いながら手をつけられなかった小さなアプリで試してください。学びをチームで共有し、他の人からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張するには、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
