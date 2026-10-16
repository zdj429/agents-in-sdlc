---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。解決されていないセキュリティ上の問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といった作業を自律的に実行するよう設計されています。作業が終わると、人間の開発者がレビューできるドラフト PR が作成されます。これにより、手間のかかる作業を任せて開発を加速し、より大きな課題に集中できます。

このレッスンでは、Copilot cloud agent について次のことを確認します。

- コード生成用の環境をカスタマイズする。
- 操作が安全に実行されるようにする。
- issue の範囲を明確にすることの重要性。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した請負業者は、ドキュメントを十分に整備していませんでした。まず、アプリケーションのエクスポートされたすべての関数に TSDoc ドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに、同じカテゴリの別のタイトルなどの関連ゲームを表示して、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。現在はブロッカーになっていますが、優先度がさらに高い issue がほかにあります。

このような作業は優先度が下がりがちですが、Copilot cloud agent に任せるのに適しています。cloud agent に非同期で作業させている間、開発者は別のタスクに集中できます。その後、Copilot の成果をレビューして、期待どおりかどうかを確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じように、バックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境をセットアップして、タスクに取りかかります。作業中または完了後に Copilot のセッションを確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、細部を見ていくと複雑さが見えてきます。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試しながら調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、それが Copilot を導く方法を紹介しました。カスタム指示の演習をすでに完了している場合は、`.github/copilot-instructions.md` の動作を確認したことがあるでしょう。まだの場合は、ここで一度目を通してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後で指示の影響を見つけられるようにします。

GitHub の Web UI または、起動済みであれば Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクションを確認し、特に TypeScript の規約と TSDoc ドキュメントコメントに関する要件に注目します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたとき、作成されるプルリクエストで TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーを確認してください。これらの指示ファイルに由来する内容は、後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

人間が作業する場合と同じように、コードを作成するときは通常、特定の環境を用意し、良好な状態で作業を始めるためのセットアップスクリプトを実行します。SWE と同様の方法で作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使用します。作業開始前に実行する[専用のセットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して環境をカスタマイズし、必要な開発ツールと依存関係を利用できるようにします。ラボをスムーズに進めるため、この設定はあらかじめ行われています。単一の Astro アプリで Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *この手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

これは他の GitHub ワークフローファイルと似ていますが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たずに、正常に動くかテストするのに便利です。

## ドキュメントを追加する

ドキュメントの重要性はよく知られていますが、ほとんどのプロジェクトでは情報が古いか、まったく整備されていません。この種の技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、これは Copilot cloud agent に依頼するのに適した issue です。バックグラウンドで必要なドキュメントを生成してくれます。後の演習で成果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に新しい PR が見当たらない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が ToDo リストを作成します。

> [!NOTE]
> Copilot が PR に ToDo リストを表示するまで数分かかることがあります。Copilot は先ほど説明したワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新後のインジケーターを表示するために、ウィンドウを更新する必要がある場合があります。

16. 実行中のセッションをスクロールして、Copilot が問題に取り組む様子を確認します。コードを調査して現在の状態を把握し、計画を考えて選択し、コードを作成する過程を確認できます。

これには数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期で実行し、他の作業に集中できるようにすることです。この機能を活用して、cloud agent に別のタスクを割り当ててから、アプリケーションに機能を追加するコードの作成に取りかかります。

## ゲーム詳細ページに関連ゲームを追加する

前述のとおり、GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザインチーム向けに関連ゲームのセクションを追加する作業が短時間で終わるとしても、その時間を別のタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使用します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく issue の最初のコメントに 👀 が表示され、Copilot が作業中であることがわかります。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent は SWE と同じように動作するため、積極的に監視し続ける必要はありません。完了したらレビューしましょう。次に、カスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について確認しました。cloud agent に issue を割り当て、非同期に作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに Copilot を利用できます。

このレッスンでは、次のことを行いました。

- コード生成用の環境をカスタマイズする。
- 操作が安全に実行されるようにする。
- issue の範囲を明確にすることの重要性。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みましょう。[Copilot cloud agent は MCP サーバーも使用できます][cloud-agent-mcp]。また、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
