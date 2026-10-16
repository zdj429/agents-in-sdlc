---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、その他のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを調べ、あらかじめ用意されたフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング issue から始め、Plan モードでアプローチを決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーで確認した根拠を評価しました。
- フィルタリング機能の変更全体をレビューし、フィルタリング PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使えるスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップでは、更新済みの `main` からそれぞれ個別のブランチを作成し、2 つの PR の節目を設けました。

1. **星評価**: 既存の `starRating` と、未評価であることを明示する表示をゲームカードに追加しました。
2. **フィルタリングと品質のワークフロー**: フィルタリング機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して関連テストを追加しました。

フィルタリングの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、すべての作業を 1 つの PR にまとめました。

## さまざまな方法で検証する

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した形式で報告しました。PR を開く前に、QA はこれらの結果と要件、テストカバレッジのレビューをまとめました。

追加するテストでは、実際に不足している部分を補います。QA を実行してテストを追加する必要がない場合もあります。マージを許可する前に、コードと根拠をレビューし、変更後は影響を受ける根拠を再確認してください。

## ベストプラクティス

Copilot に提供するコンテキストとツールは、作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し可能なタスクを定義し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。作成前にアプローチを検討するには **Plan**、範囲を絞った変更の経過を確認するには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。作成するもの、その理由、実装方法を明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを完了しました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題について相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させる最善の方法は、使い続けることです。本番コード、趣味のコード、何年も作ろうと思っていた小さなアプリなどに利用してみましょう。チームと学んだことを共有し、ほかの人からも学びましょう。そして、いつものようにドキュメントを確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進んでください。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
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
