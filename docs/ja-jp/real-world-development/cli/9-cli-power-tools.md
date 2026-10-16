---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールと同じように、GitHub Copilot CLI にも操作用のスラッシュコマンドが多数用意されています。これらのコマンドでは、高度な機能、内部情報、追加の構成オプションを利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ほかの便利なコマンドも確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する仕組みを確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを完了しました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、その他の機能を確認します。

## Copilot CLI のコンテキストを確認する

規模が大きい、または複雑なタスクでは、モデルの最大コンテキストウィンドウに達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って自分で状態を確認したり、圧縮したりすることもできます。

1. Codespace に戻ります。Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように割り当てられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話がすでに短い場合は、あまり変化しないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI はコンテキストを自動的に圧縮します。圧縮のタイミングを自分で選ぶ場合は `/compact` を使います。関連のないタスクに切り替えて新しい会話を始めたい場合は、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意分野があり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルの一覧を表示して使うモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを続けて使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで共同作業し、学んだことを共有すると、AI ツールの活用を改善できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown にエクスポートする `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容を公開しないでください。

## 任意: 作業をスケールアウトまたは委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに使える次のコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分け、並行して実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プルリクエストを開くことがある Copilot cloud agent にタスクを送信できます。

これらのコマンドは、追加の worktree やリモート作業を発生させる可能性があるため任意です。試す前に、範囲の明確な新しいタスクを始め、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業をさらに学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定やセッション共有、内部で行われている作業の確認ができます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する仕組みを確認しました。
- `/model` を使って利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュコマンドと機能があります。学んだことを[振り返り][next-lesson]、次の学習に進みましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
