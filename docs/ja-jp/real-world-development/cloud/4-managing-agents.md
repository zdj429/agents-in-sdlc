---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、ユーザー体験の改善や機能追加を目的とした 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作するように設計されていますが、タスクの進行状況を監視することも大切です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。その 1 つが、GitHub.com の[agents ページ][agents-page]です。この mission control では、オープンなプルリクエスト (PR) を持つすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションを誘導したりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 進行中のセッションを誘導し、追加機能を依頼します。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加することにしました。サイトのスタイルを更新し、切り替え機能を追加する作業をすでに進めているので、ライトモードも含めるのが自然です。エージェントの作業を誘導し、ハイコントラストモードに加えてライトモードも追加してもらいましょう。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインに表示されるタスク一覧を確認します。Copilot に割り当てた次のタスクが表示されます。
    - コードベースのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択して、Copilot が実行した作業を確認します。次のような操作が表示されます。
    - リポジトリからコードをチェックアウトする。
    - Copilot が作業する環境を作成する。
    - MCP サーバーをセットアップする。
    - 割り当てられたタスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や選択した進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認してください。PR や変更されたファイルを確認して、さらに監視できます。

## cloud agent を誘導する

実行中のタスクを確認できたので、ハイコントラストモードに取り組んでいる Copilot に、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードの追加を示すセッションを選択します。Copilot が付けた名前や作業状況によって、正確なタイトルは異なります。

    ![mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。次のようなメッセージが表示されたら、作業を開始したことがわかります。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot agents ページの cloud agent タスク画面。「Steer active session while Copilot is working」ダイアログが強調表示されています](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り入れる様子を確認します。

## Copilot の作業を待つ

前と同じように、Copilot は更新されたタスクに取りかかります。メッセージを送信した時点で実行していたステップを完了した後、新しい依頼を作業に取り入れます。

今回も数分かかるため、ここまでに学んだことを振り返る時間にしましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視し、誘導するための中心的な場所である Copilot agents ページを確認しました。この mission control では、実行中と完了済みのタスクを追跡し、作業内容を確認して、進行中のタスクの範囲を変更したり追加のガイダンスを提供したりできます。

この演習では、次のことを行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションを誘導して、追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、Copilot が作成した[プルリクエストを反復改善][next-lesson]しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復改善 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
