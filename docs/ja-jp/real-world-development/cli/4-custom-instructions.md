---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加してフィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、作成するコードの内容だけでなく、どのように構成するかも指定できます。フィルター機能を作成したので、Copilot が使用した指示を確認し、ドキュメント標準を追加して、コードに適用しましょう。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる仕組みを確認します。
- コーディング標準が守られるよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントではコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、`db` 引数を注入できる場合はそれも含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更された場合はコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、強調したプラクティスに沿ってタスクを実行するために必要な情報を Copilot に伝えます。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと好みを Copilot に伝えられます。コーディング規約、ライブラリ、コメントの書き方などを指定すれば、より適切な提案やコードスニペットを得られます。リポジトリ全体、または特定のファイル種別のタスクに対して指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定の言語、ファイル種別、タスク向けのガイドラインを提供します。

> [!NOTE]
> その他の指示形式とサポート状況は環境によって異なります。特定の形式を使う前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターターには指示ファイルが用意されています。変更の影響を確認するため、まず現在の内容を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディングガイダンスを確認します。このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示が適用されるファイルを決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分割し、Copilot が参照する例を指定できます。

## チームのガイダンスに合わせて指示ファイルを更新する

現在のファイルはよい出発点ですが、まだ不足があります。中核となる `copilot-instructions.md` ファイルを変更し、新たに生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 次の内容を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。機能に関するコンテキストを保ったまま新しい指示を読み込ませるため、指示を保存した後、終了してフィルタリングの会話を再開します。

1. フィルタリングの会話がまだ開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は、フィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新されたガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがあり、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる仕組みを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次に、再利用可能な [quality-checks スキルをカスタマイズして実行][next-lesson]し、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
