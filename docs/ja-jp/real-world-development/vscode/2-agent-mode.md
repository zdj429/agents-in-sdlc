---
title: "演習 2 - Copilot Agent Mode で新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さな更新でも、通常は複数のファイルを変更し、テストなどの操作を実行する必要があります。開発者は必要なファイルを見つけ、変更し、テストを実行し、問題をデバッグし、変更漏れのファイルを探して、さらに更新するという作業を繰り返します。

このような場合に Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に動作します。開発者と同じように、まず既存のプロジェクト構造を調査し、必要なファイルを更新し、テストなどのタスクを実行して、見つかった誤りを自動的に修正します。Agent Mode を使ってサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途を想定しています。Agent mode は IDE 内で作業し、素早くフィードバックをやり取りします。Cloud agent はチームのメンバーのように非同期で動作し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、連携した変更を行う方法。
- UI とデータレイヤーにまたがる新機能を GitHub Copilot Agent Mode で実装する方法。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューする方法。

## シナリオ

ゲームの一覧が増えてきたので、ユーザーがカテゴリと publisher で絞り込めるようにします。前の演習で publishers ヘルパーを追加しました。ここでは Copilot Agent Mode を使って、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認して現在の機能を理解しましょう。

この Web サイトは、開発者向けテーマのボードゲームを扱うクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を表示できます。サイト全体は単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは frontmatter から Drizzle ORM を使ってローカルの SQLite データベースを直接照会するため、別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、必要なインタラクティブ機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で実装します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習後の状態は `main` です。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルター機能の作業用にブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` のブラウザーウィンドウを開くように求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択するときに、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押したままにします。

> [!NOTE]
> Codespace ターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーを実行している Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明の一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、publisher、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始してください。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能で拡張しましょう。まずバックログを確認します。テンプレートからプロジェクトをセットアップしたときに issue のバックログが自動作成されています。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンで **Agent** を選択します。**Agent** エージェントは自律的に変更を計画して複数ファイルに実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェントピッカーのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作者は、特定のモデルを推奨しているわけではありません。ワークショップの作成には Claude Sonnet 4.5 を使用したため、手順にも同じモデルを記載しています。提案を比較的一貫させて円滑に進めるためですが、LLM は確率的であるため、ワークショップと異なる提案が出る場合があります。これは正常で想定される動作です。

> [!NOTE]
> LLM は確率的に動作するため、Copilot が別の MCP コマンドを使用することがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーに役立つと考えた順に項目を優先付けしていることを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot に作業のコンテキストを提供する指示ファイルを確認しましょう。Web サイトに機能を追加する方法を示す [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全般的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト可能性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから、プロジェクト内の別の指示ファイルやファイルを参照できます。パスは指示ファイルからの相対パスです。これにより、指示を再利用し、複雑な指示を小さく管理しやすい内容に分割して、例やテンプレートを提示できます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データレイヤー (`src/lib/`) に残りのフィルタリングロジックを追加または調整します。
- フィルター動作のテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行し、成功することを確認する必要があります。Copilot Agent Mode にこれらの作業を任せましょう。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例のスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が引き続き選択されていることを確認します。
5. バックログにある関連 issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを特定する様子を確認します。データレイヤーのヘルパー、UI、テストを探し、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調査している様子のスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数の作業を行います。タスクやコードベースの複雑さによっては数分かかる場合があります。作業中、コードエディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業が完了するとすべての変更に対して **Keep** または **Undo** が表示されるので、作業中に選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テストを実行してよいか確認する Copilot Chat ペインのダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したりテストが失敗したりする場合がありますが、問題ありません。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーが見つからなくなるまで作業します。

   ![Copilot Agent Mode の完了したチャットセッションのスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに新機能を追加する方法を学びました。

- UI とデータレイヤーにまたがる新機能を GitHub Copilot Agent Mode で実装しました。
- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを特定して、連携した変更を行いました。
- Copilot Agent Mode が生成した変更とテストを、コードベースにマージする前にレビューしました。

次に [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使って、Copilot にページネーションの実装を依頼してください。次の点を検討します。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従います。
- ユーザーがページサイズを選択できるようにするか、固定値を使用するかなど、ページネーションの方法を検討します。
- 希望する実装に必要なガイダンスを、Copilot にプロンプトで伝えます。
- Copilot に変更を依頼してコンテキストを追加するなど、反復作業が必要になる場合があります。これは Copilot を使う際の通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 用途と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
