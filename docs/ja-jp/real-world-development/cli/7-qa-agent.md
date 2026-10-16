---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー内のフィルター機能を確認しました。次に、これらの機能を明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリング issue に照らして調査結果をレビューします。

## シナリオ

Tailspin Toys は、プルリクエスト (PR) を開く前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタムエージェントを使うと、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的なバージョンです。プロファイルでエージェントの目的、指示、利用可能なツールを説明します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し可能なチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、Copilot がこれらの機能を使って要件を評価し、結果を報告する方法を指定します。それぞれの機能を置き換えたり、新しい会話を必要としたりするものではありません。

## QA プロファイルを作成する

機能の PR を開く前に、Copilot に再利用可能な QA プロファイルを作成してもらいます。このプロファイルでは、QA が実行するチェックと、従うべき制約の両方を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のプロンプトで新しいカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約を反映したことを確認します。これにより、検証だけをしたい機能に不完全または範囲の広すぎるエージェントが変更を加えるのを防ぎます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略できますが、指定するとエージェントの表示名がわかりやすくなります。
3. プロファイルの指示を読み、QA が要件を基準に作業を始め、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードの変更前に確認を求め、変更のコミットやプルリクエストの作成を行わないことを確認します。
5. これらの責任や制約が不足している場合は、続行する前に既定のエージェントにプロファイルを修正してもらいます。

## issue を基準に QA を実行する

Copilot CLI は会話開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、フィルタリングの会話を再開し、QA を選択して、コンテキストに含まれている issue と計画での決定を利用できるようにします。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。上記のコマンドを実行すると有効になります。

2. 次のプロンプトで QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、エージェントのレポートを確認します。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに追加し、その結果をレビューしました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリング issue に照らして調査結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次に、それらを[機能 PR にまとめて Agent Merge を使用します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
