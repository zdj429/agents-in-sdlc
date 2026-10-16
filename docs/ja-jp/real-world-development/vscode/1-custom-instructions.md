---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合は、Copilot が参照できるようにガイダンスを用意します。[指示ファイル][instruction-files]を使うと、どのようなコードを作成するかだけでなく、どのように構成するかも伝えられます。フィルター機能を作成したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用しましょう。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準がエージェントに伝わる仕組みを確認します。
- フィルタリング用の最初のデータスライス (publishers ヘルパー) を、現在の指示を適用して生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が適用されることを確認します。
- 指示とフィルタリング用の変更を `main` にコミットし、Copilot が次の演習で更新されたガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードが、指定した標準の一部と異なる場合があります。Copilot は非決定的に動作します。この演習の目的は、出力を一字一句同じにすることではなく、指示を更新した後に動作の**傾向**が変わることを確認することです。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにして、現代的な見た目にします。
- コードには TSDoc ドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、強調したプラクティスに沿ってタスクを実行するために必要な情報を Copilot に伝えます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと好みを Copilot に伝えられます。コーディング規約、ライブラリ、コメントの書き方などを指定すれば、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定のファイル種別のタスクに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関係する、技術スタック、開発対象の概要、ベストプラクティスなど、プロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイル種別に使用できます。TypeScript などの言語、UI コンポーネントやユニットテストの作成といったタスク向けのガイドラインを指定できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成についてすべて説明することはこのワークショップの範囲外ですが、サンプルプロジェクトには代表的な方法が示されています。基本的には、次のようにします。

- `copilot-instructions.md` には、開発対象やプロジェクト構成、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルは、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別やタスクに応じた指示に使います。
- 自然な言葉で、明確なガイダンスを書きます。コードの良い例と悪い例も示してください。

指示ファイルの作成方法は 1 つではありません。AI のほかの活用方法と同じように、環境やシナリオに合わせて試し、調整してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、多様なタスク向けのファイルがあります。
>
> Copilot に指示ファイルの生成を依頼することもできます。各環境で手順は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、または Copilot CLI の `/init`)。関連するレッスンで、その環境の方法を紹介します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読んでみましょう。中核となる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内にある **GitHub Actions Workflows** のガイダンスにも注目してください。これらは Copilot を使うあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示が適用されるファイルを決める glob です。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象になります。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや既存のプロジェクトファイルへのリンクがあります。大きな指示を小さく再利用可能なファイルに分割し、コード生成時に参照する例を指定できます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で、TSDoc ドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 次のコマンドでブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始してください。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するために、まず現在の指示ファイルを使ってプロンプトを送ります。後で指示を更新し、同じプロンプトをもう一度送って違いを確かめます。

1. Copilot が必要なコンテキストだけを取得できるよう、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェントピッカーのスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードを変更します。多くの場合、ヘルパーファイルとテストの両方が変更されます。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、publisher の型付き配列を返す型付き関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードには、TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加することがあります。その場合も問題ありません。指示を更新した後の**一貫性**の改善を確認することが目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。このため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に、次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc ドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル種別、タスク向けに `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメントコメントのルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をリセットします。
2. `src/lib/publishers.ts` を選択し、Copilot が正しいファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で利用する実際のコードに適用しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内の他のファイルと同じように、ソース管理で管理します。指示とブランチをリポジトリにコミットしてプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. 指示の変更と、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、そのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動でも作成できますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼しましょう。Copilot に issue を探させ、issue を完了する PR を作成してマージします。

> [!NOTE]
> この演習では、初めて Copilot に GitHub 上で操作させます。プロジェクトのテンプレートには GitHub ツールがあらかじめ接続されています。Copilot が初めてツールを使う際、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従ってください。この接続 (Model Context Protocol) の仕組みは後の演習で説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイル更新に関連する issue を探し、指示の更新と新しい publishers ヘルパーの基盤の両方を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が代わりに GitHub の操作を実行するための **Allow** を求めたら、コマンドを確認して適切な場合に許可します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub の操作やシェルコマンドを実行するための **Allow** を求めたら、コマンドを確認して適切な場合に許可します。

GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を適用した状態でコード生成プロンプトを送信し、Copilot の生成結果を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加しました。
- 同じプロンプトを再実行し、提案コードに新しい標準が適用される様子を確認しました。
- 次の演習で利用できるよう、指示とヘルパーの基盤を `main` にコミットしました。

次に、Copilot がコードベース全体に機能を追加する際に、[agent mode][next-lesson]で指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
