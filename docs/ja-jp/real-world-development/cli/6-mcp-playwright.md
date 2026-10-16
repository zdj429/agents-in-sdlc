---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くこと以外にも開発にはさまざまな作業があります。データや外部サービスを扱ったり、Copilot がさらに自動化できるようにしたりする必要があります。MCP サーバーを使うと、CLI に組み込まれた機能に加えて、Copilot にツールやサービスを提供できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどのように使うかを理解します。
- Playwright MCP サーバーが利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように Web サイトを利用できるようにすると、変更が期待どおり動作する確信を高め、自動化をさらに進められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信する方法を提供します。MCP を使うと、AI エージェントはリアルタイムに外部のツールやサービスと連携し、最新情報にアクセスして、ユーザーに代わる操作を実行できます。

これらのツールやリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介して利用できます。各 MCP サーバーは、AI エージェントが利用できる異なるツールとリソースの集合を提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステムの発見と貢献を促す [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーはプロジェクトの他の依存関係と同じように扱ってください。使う前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP を構成している場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力して <kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright を使って Copilot に機能を確認させる

これまでは機能が期待どおり動作することを手動で確認しました。今度は Copilot で利用できる Playwright MCP サーバーを使って、同じ検証を行いましょう。

1. 次のプロンプトを使って、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、MCP サーバーの使用方法を自動的に判断するため、明示的に指示する必要はありません。ただし、利用するサーバーがわかっている場合は、方向を示しておくと結果の一貫性が高まり、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するため、さまざまな手順を実行する様子を見ます。
3. レポートを読み、すべてが期待どおり動作していることを確認します。

Copilot はサーバーを起動し、Playwright を使って Web サイトを操作し、サーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどのように使うかを学びました。
- Playwright MCP サーバーが利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次に、スキルとブラウザーツールを専門的な役割にまとめた [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
