---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、専門の AI アシスタントを作成できます。リポジトリ内の `.github/agents` フォルダーに Markdown ファイルを配置してエージェントを定義し、焦点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を与えて、特定の作業をより効果的に実行できるようにします。チームの専門知識を再利用可能なエージェントとして共有できます。たとえば、[WCAG][wcag] 準拠を適用するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定める Markdown プロンプトを記述します。

### カスタムエージェントと agent skills の比較

カスタムエージェントと [agent skills][agent-skills-concept]には、機能が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に操作方法を伝えます。最もわかりやすい違いは、**カスタムエージェントは作業者で、スキルはツール**だということです。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業を進める中でスキルや他のエージェントを統括するように設計されています。このラボのアクセシビリティ用カスタムエージェントは、アクセシビリティガイドラインに照らしてサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフローを進めるスキルや、テストを実行・管理するスキルを呼び出すことができます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの活用方法と同じように、環境やシナリオに合わせて試し、調整してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次のことを行います。

- カスタムエージェントの定義方法を確認します。
- タスクをカスタムエージェントに割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくいという意見がありました。このアクセシビリティの課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントを使って、この機能を生成します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用カスタムエージェントはすでに用意されています。その内容を確認し、Copilot をどのように導くのか見てみましょう。

Codespace に戻り、アクセシビリティ用カスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションに目を通し、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責任。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違いの一覧。

## issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進行状況を確認し、追加のガイダンスを提供して作業の方向を変えられます。まず、ハイコントラストモードの作成を Copilot に割り当てましょう。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントとアクセシビリティエージェントが選択されている cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは特定のタスクや分野向けに調整した AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントとして共有し、Copilot がより効果的に作業できるようにします。

この演習では、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- タスクをカスタムエージェントに割り当てました。

Copilot がハイコントラストモードを実装している間に、[mission control からエージェントセッションを監視して誘導][next-lesson]しましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを利用するための準備][org-custom-agents]
- [Enterprise でカスタムエージェントを利用するための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
