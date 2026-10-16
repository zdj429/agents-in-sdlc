---
title: "レッスン 2 - 星評価を追加する: 手早くできる改善"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送されたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、プロジェクトに最初の変更を加えましょう。小さな変更から始めます。ゲームデータには星評価がすでにありますが、ホームページのゲームカードには表示されていません。エージェントに評価を表示させ、変更をレビューし、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- 転送されたブラウザーでアプリを実行して変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードには、タイトル、カテゴリ、publisher、説明しか表示されません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。最初のセッションにぴったりな、小さく独立した変更です。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む場所です。Copilot app とは異なり、通常の CLI 会話は専用の worktree を作成せず、ターミナルでチェックアウト済みのリポジトリと Git ブランチを使います。保存した会話に戻ると、同じ内容を引き続き利用できます。ファイルとブランチは通常の Git の状態としてディスクに保持されます。

会話では、プロンプトとエージェントの応答、ファイルを調査して編集する際のツールの動作、`/diff` で確認できる変更の 3 つが表示されます。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能の実装を依頼します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. まだ Copilot を起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. `/new` スラッシュコマンドを入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調査し、ゲームの詳細を表示するファイルを見つけて、必要なコードを作成します。Copilot CLI を使って新機能を追加できました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はすべてマージ前にレビューします。Copilot CLI で変更を確認しましょう。

1. `/diff` を入力し、変更された各ファイルを確認します。
2. 星評価がある場合はゲームカードに数値が表示され、`starRating` が `null` の場合は `No rating yet` と表示されることを確認します。
3. テストで両方の状態を確認していることを確かめます。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot を含め、生成 AI ツールは決定論的ではなく確率的に動作するため、コードは正確には同じにならない場合があります。特定の実装を期待するのではなく、動作をレビューしてください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動させて、Codespaces から転送されたブラウザーで更新されたユーザーインターフェイス (UI) を確認しましょう。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を利用可能と通知したら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できたので、新しいコードをプロジェクトに取り込むプルリクエスト (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR の作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI に表示された URL を選択するときに、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押したままにして PR を開きます。
4. 変更されたファイルとチェックを確認します。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースしました。

- 機能ブランチで Copilot の会話を開始しました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- 転送されたブラウザーでアプリを実行し、星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次に、[フィルタリング issue から始め、Plan モードと Autopilot モードを使って][next-lesson]大きな機能を作成します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
