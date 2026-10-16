---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で issue からフィルタリング機能を作成し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。しかし、大きな変更には、より堅牢なプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って動作するため、適切なものを適切な方法で構築できます。このレッスンから、エージェント主導の一般的な開発プロセスを学びます。issue から新機能を作成し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能のワークフローを通して、同じ会話とブランチを使います。通常は作業対象のファイル種別ごとにブランチや PR を分けますが、ここでは主要な概念に集中できるよう簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP で検証して、QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者はカテゴリや publisher でゲームを絞り込む必要があります。バックログの issue に機能は記載されていますが、カテゴリを組み合わせる方法など、コーディング前に決めておく必要がある詳細があります。Plan モードでこれらの決定を行い、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディングエージェントを開発ワークフローに導入しても、基本は変わりません。むしろ、基本がより重要になります。多くの開発者は、次のような手順で作業します。

1. 実装内容を説明する issue から始めます。
2. 作成するものを計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードがレビューされ、継続的インテグレーションの処理が成功したら、コードをマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、ほとんどは上記の流れに沿っています。

この標準的なアプローチに従うことで、AI が生成したコードも要件を満たし、手作業で書いたコードと同じ審査プロセスを経るようにします。

## 会話モード

**会話モード**は、エージェントの自律性の度合いを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと共同で作業します。エージェントは変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントはまず計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントはコードを書き、テストを実行し、タスクが完了するまで反復して自律的に作業します。

Plan モードから始めて計画をレビューし、その後 Autopilot で実装します。

## issue から開始する

フィルタリング作業を開始する前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続行します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使って GitHub の issue やプルリクエスト (PR) をコンテキストに追加します。

## フィルター機能を計画する

計画を立てると、Copilot に作業を任せる前に、機能実装やタスク実行のアプローチを定義できます。複雑な作業では、事前に時間をかけて計画することをお勧めします。Plan モードに切り替えて、Copilot に計画の作成を依頼しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にモード **Plan** が表示されていることを確認します。
2. 前の手順で追加した issue への参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot が計画を作成します。まずプロジェクトを調査し、その後、最適なアプローチを検討します。

3. フィルタリング機能の動作について、Copilot から質問される場合があります。希望に合わせて回答してください。正解はありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. スクロールして計画を確認します。
6. 判断と一致しない部分があれば、Copilot に計画の修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたので、実装に進みましょう。Autopilot を使って Copilot に作業させます。

Autopilot を使うと、Copilot は完了したと判断するまで問題への対応を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様のラベルが付いた選択肢を選びます。
2. プロンプトの下にモード **Autopilot** が表示されていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行しながら反復する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。事前に計画を確認してください。Copilot から依存関係の不足やポートの競合を報告された場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードの生成後は、他のコードと同じようにマージ前のレビューが必要です。コードをレビューし、サイトを実行して正しく動作することを確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モードインジケーターに **Plan** や **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. 結果を issue および計画中に決定した内容と照らし合わせます。
4. コードレビューが完了したら、<kbd>Esc</kbd> を押して差分ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を確認する

コードはよさそうですが、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces がポート `4321` を利用可能と通知したら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリの絞り込み、publisher の絞り込み、各種組み合わせを試します。
4. リセットと結果なしの場合の動作が、issue と決定内容に一致することを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次に、[カスタム指示][next-lesson]を使って、コードが文書化されたプラクティスに従うようにする方法を学びます。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
