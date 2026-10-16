---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように実行するかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの「前/後」を比較しません。Copilot cloud agent は GitHub issue に対して非同期に動作するため、リアルタイムで並べて影響を確認するのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューする際に、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにして、現代的な見た目にします。
- コードには TSDoc ドキュメントコメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、強調したプラクティスに沿ってタスクを実行するために必要な情報を Copilot に伝えます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと好みを Copilot に伝えられます。コーディング規約、ライブラリ、コメントの書き方などを指定すれば、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定のファイル種別のタスクに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の指示ファイルです。Copilot に送る多くのチャットや CLI リクエストに関係する、プロジェクトレベルの情報を記載します。技術スタック、開発対象の概要、ベストプラクティス、全般的なガイダンスなどが該当します。
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
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトに含まれる指示ファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、多様なタスク向けのファイルがあることに気付くでしょう。
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
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の中にある、入れ子の **GitHub Actions Workflows** ガイダンスにも注目してください。これらは Copilot を使うあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示が適用されるファイルを決める glob です。たとえば `**/*.test.ts` に一致する TypeScript テストファイルが対象になります。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示を小さく再利用可能なファイルに分割し、コード生成時に参照する例を指定できます (ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で、TSDoc ドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## リポジトリ標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントは TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーで記述する、というプロジェクト全体のルールを追加します。後でこのハーネスの issue を Copilot に割り当てたとき、cloud agent がこのルールを適用します。

編集を始める前に、作業用ブランチを作成します (cloud agent は issue の対象ブランチから指示を読み取りますが、ここでは編集を適切にコミットします)。

1. Codespace のターミナルで、次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案を改善するため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはまだありません。このため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に、次の Markdown 行を追加して、ファイルコメントヘッダーと TSDoc ドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル種別、タスク向けに `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメントコメントのルールのようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

Cloud agent は issue が対象とするブランチから指示ファイルを読み込みます。次の演習で Copilot に issue を割り当てると、Copilot は `main` から分岐します。そのため、cloud agent が変更を適用できるよう、指示を `main` に取り込む必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で割り当てた issue に取り組む際、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合は、ブランチを作成せず `main` に直接コミットできます。ブランチを使う手順は、実際のプロジェクトでの作業方法を再現するためにあります。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからどのようにコンテキストを取得するかを確認し、リポジトリ全体に適用する標準を追加しました。このハーネスの残りで cloud agent が作成するプルリクエストを確認すると、追加した標準が適用されていることがわかります。

次に、Copilot cloud agent に [最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けの優れたカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
