---
title: "演習 5: GitHub Copilot の作業を反復改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加を目的として、GitHub Copilot に複数のタスクを依頼しました。コードへのドキュメント追加、デザインチームが改善案を検討できる関連ゲーム機能の作成、ハイコントラストモードとライトモードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを提供して成果を改善しましょう。

### シナリオ

繰り返し説明してきたように、生成 AI が加わってもソフトウェア設計や DevOps の基本は変わりません。生成されたコードを必ずレビューし、通常の DevOps プロセスに沿って作業します。チームでレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監督なしで非同期にタスクを実行するため、安全性を確保する制約が設けられています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセスはコード用のブランチ**のみ**に限られます。
- cloud agent は GitHub Actions 内で実行され、独立した一時的な環境を作成して作業します。
- GitHub Actions ワークフローを実行するには、必ず人間の承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも含まれます。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。Copilot は既定で外部 MCP サーバーへの接続など、外部リソースへのアクセスが制限されているため、これは**想定どおり**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が付いています。その場合は Copilot の作業が完了するまで待ってください。数分かかることがあるので、その間に休憩したり、これまでに学んだことを振り返ったりしてもよいでしょう。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc ドキュメントコメントやその他のドキュメントを含む、更新されたコードを確認します。変更内容は状況によって異なります。

    内容を確認するときは、Exercise 2 の最初に確認した指示ファイルに由来する TSDoc ドキュメントコメント、TypeScript の規約、ファイルコメントヘッダーに注目してください。

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストでの Copilot との作業は一方通行ではありません。チームのメンバーと同じように PR のコメントやコード行へのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は非決定的なので、依頼文を特定の形に指定することはできません。たとえば、次のような更新を Copilot に依頼できます。

- 各コードファイルの先頭に、ファイルの概要を示すコメントヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc ドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の案など、コードベースに必要なドキュメントの更新を依頼するコメントを追加し、他のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブを選択して変更を確認します。

満足できるまで、反復して変更を依頼できます。納得できたら、ドラフト PR をレビュー可能な状態に変更し、`main` ブランチにマージします。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com でリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. 承認待ちのワークフローがあることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名、たとえば **copilot/fix-8** に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、cloud agent に更新を依頼して反復作業を進められます。たとえば、表示する関連ゲームの数の変更や、コメントヘッダーと TSDoc ドキュメントコメントの追加を依頼できます (この issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。これまでと同様に、**Conversation** タブで新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントを使って実装した機能を確認します。この PR には、Exercise 3 で依頼したハイコントラストモードと、Exercise 4 の mission control で追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com でリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルには **[WIP]** が付いています。その場合は Copilot の作業が完了するまで待ってください。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装をレビューし、特に次の点に注目してください。
   - 各モードを切り替える UI コンポーネント
   - ユーザー設定を保存するためのローカルストレージの使用
   - ハイコントラストモードとライトモードの CSS やスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認し、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. 承認待ちのワークフローがあることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    続いてアプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装を導いたことに注目してください。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent を使うと、実際のバックログ項目を任せて、その間に別の作業に取り組めます。この習慣を身に付けるため、リポジトリに issue をいくつか追加して Copilot に割り当ててみましょう。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンを通して、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認**し、後で cloud agent の出力への影響を確かめられるようにしました。
- **Copilot cloud agent に issue を割り当て**、環境を設定し、計画を立て、非同期で作業する様子を確認しました。
- **カスタムエージェントを作成して使用**し、ハイコントラストモードとライトモードを追加しました。
- **Copilot agents ページを mission control として使い**、作業中のアクセシビリティセッションを監視して誘導しました。
- **cloud agent のプルリクエストをレビューして反復改善**し、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスにも取り組むと、ここで学んだ内容をさらに深められます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から離れずに Copilot Chat の agent mode と MCP 統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、agent skills、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` ブリッジを使って、ターミナルからレビュー済みの変更を 2 件提供します。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテストカバレッジの issue を cloud agent に割り当てます。
- 別の分野向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
