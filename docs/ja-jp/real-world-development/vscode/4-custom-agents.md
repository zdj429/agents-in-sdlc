---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを配置してエージェントを定義し、焦点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を与えます。チームの専門知識を再利用可能なエージェントとして共有できます。たとえば、[WCAG][wcag] 準拠を適用するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定める Markdown プロンプトを記述します。

### カスタムエージェントと agent skills の比較

カスタムエージェントと [agent skills][agent-skills-concept]には、機能が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に操作方法を伝えます。最もわかりやすい違いは、**カスタムエージェントは作業者で、スキルはツール**だということです。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業を進める中でスキルや他のエージェントを統括します。このラボのアクセシビリティ用カスタムエージェントは、アクセシビリティガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフローを進めるスキルや、テストを実行・管理するスキルを呼び出すことができます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの活用方法と同じように、環境やシナリオに合わせて試し、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくいという意見がありました。このアクセシビリティの課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用カスタムエージェントはすでに作成されています。内容を確認し、Copilot をどのように導くのか見てみましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します (Exercise 3 のフィルター PR とは分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションに目を通し、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責任。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違いの一覧。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` に定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールに合わせてチャットセッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始してください。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

1. まだ選択されていない場合は、Chat ビューのエージェントのドロップダウンから **Agent** を選択します。

   ![Chat ビューのエージェントピッカーのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. チャットビュー下部にあるエージェントのドロップダウンを選択します (アクティブなエージェントが表示されています。既定値は **default** です)。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティエージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調査し、変更を提案してプロジェクトファイルを編集します。各編集はファイルパスと差分とともにチャット内に表示されます。
6. エージェントが編集するファイルの一覧は、**Files changed** インジケーターに反映されます。

> [!NOTE]
> この作業には数分かかります。Copilot はリポジトリに実際の変更を加え、Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集します。

次の演習で、この作業の進行中にレビューし、エージェントを誘導します。

## まとめと次のステップ

特定のタスクや分野向けに調整された AI アシスタントである、GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能なエージェントとして共有し、Copilot を効果的に導けます。

この演習では、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat の agent mode でカスタムエージェントを使いました。

次に、進行中の作業をレビューし、同じセッションにライトモードの切り替えを追加して、[エージェントの作業を監視して誘導][next-lesson]します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
