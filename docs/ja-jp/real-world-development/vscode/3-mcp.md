---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

Agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで正しく動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルター機能を手動でテストさせます。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode がエディターの外部にも接続できるようになると、活用の幅が広がります。Model Context Protocol (MCP) を使うと、Copilot は外部のツールやサービスと標準的な方法でやり取りできます。

![Agent mode の内部と、コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP を使うと、AI エージェントはリアルタイムに外部のツールやサービスと連携し、リソースから最新情報を取得したり、ツールを使ってユーザーに代わる操作を実行したりできます。

これらのツールやリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介して利用します。MCP サーバーは AI エージェントと外部ツール (既存の API や NPM パッケージなどのローカルツール) との通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースの集合を提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリ管理用の API を利用できます。AI エージェントから新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントから Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにもさまざまなツールやリソースを提供する MCP サーバーがあります。GitHub は、エコシステムの発見と貢献を促す [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティに関しては、MCP サーバーをプロジェクトの他の依存関係と同じように扱ってください。MCP サーバーを使う前にソースコードを注意深く確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使用し、機密リソースや操作へのアクセス権を付与する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーはすでに構成されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot はバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うので、ローカルにインストールする必要はありません。VS Code が HTTP で接続し、Copilot が最初にツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるようになり、機能のテストに利用できます。

1. `.vscode/mcp.json` の `github` と並ぶ位置に `playwright` エントリを追加します。ファイルは次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを使うと、Playwright は画面を表示せずにブラウザーを実行します。デスクトップ画面がない Codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright を使用しているため、必要なブラウザーは通常すでにインストールされています。後で Copilot からブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot が初めてサーバーのツールを使うとき、VS Code がサーバーを起動し、信頼するかどうかを確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** アクションが表示されます。これを選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか尋ねられたら、設定を確認して信頼するよう選択し、サーバーを起動します。

手動での起動は任意です。実行しない場合、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼するかを確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot は実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開いて移動させ、フィルターを適用して結果を読み取らせ、確認内容をまとめてもらえます。会話を離れずに機能が期待どおり動作することをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使って操作します。ボタン、リンク、リスト項目などの構造化されたラベル付き要素を使ってエージェントが推論します。支援技術と同じ方法を使うため、簡単な機能テストでアクセシビリティもある程度確認できます。

サーバーを接続してアプリを実行した状態で、次の手順に従って、作成したフィルター機能をテストするよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件と照らし合わせて要約を確認します。問題があれば、プルリクエストを開く前に追加の質問をしたり、コードの修正を依頼したりしてください。

> [!NOTE]
> このテストには `http://localhost:4321` でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が初めて Playwright MCP サーバーを使うときにブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行するよう依頼して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームがレビューできるようにプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを作成します。

   ![Copilot が行った変更を示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストを作成するには、github.com や GitHub コマンドラインインターフェイス (CLI) などいくつかの方法があります。ここでは GitHub Copilot を使い、関連する issue を見つけて関連付けた PR を作成してもらいます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるように、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報の取得と操作に必要なタスクを実行できるようにします。GitHub ツールを初めて使用するときは GitHub の認証が求められる場合があります。画面の指示に従ってください。
5. Copilot が issue を検索して適切なものを見つけ、PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動でテストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しました。MCP は Copilot を拡張する方法の 1 つです。次に、アクセシビリティレビューなどに特化したタスクを効率化するため、[カスタムエージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
