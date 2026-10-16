---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは 1 つのブランチに保存されています。すべてをまとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は**Agent Merge**にプロセスを管理させます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔に進めるため、フィルタリングと品質に関するすべての作業を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルを自動化する仕組みを学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローを通じて、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys は、マージの承認権限を開発者の管理下に保ったまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プルリクエストのマージに必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど) に取り組み、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることができます。有効にする前に、作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードがすべて完成したので、内容をレビューして PR を作成し、Agent Merge に残りのプロセスを管理させます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 選択肢の一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーション処理を監視します。すべて成功すると、マージが実行されます。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須レビュー、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合は、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの複数の部分を自動化しました。

- Agent Merge とは何か、マージのライフサイクルを自動化する仕組みを学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次に、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ [Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
