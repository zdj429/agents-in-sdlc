---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、開発にはさまざまな作業があります。手動でコードの動作を確認し、指示ファイルで標準に従うようにできました。では、テストや lint、継続的インテグレーション (CI) の他の作業はどうでしょうか。

このような作業には**agent skills**が適しています。スキルを使うと、こうした操作の実行方法を Copilot が理解できます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニットテスト、lint、型チェックに `quality-checks` スキルを使います。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skills は、エージェントが必要に応じて読み込む、再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名が付いたフォルダーと、その中の `SKILL.md` という Markdown ファイルで構成されます。Markdown にはスキルの名前と説明のフロントマター、概要、呼び出すタイミングのガイダンスを記載します。スキルのフォルダーには、スクリプトやその他のリソース用のサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルはプロジェクトの `.github/skills` フォルダーに配置して、チームで共有・再利用できるリポジトリアセットにできます。また、ユーザーのスキルフォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

ユニットテスト、lint、型チェックを実行するために Tailspin Toys チームが作成した `quality-checks` スキルを見てみましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明は Copilot がスキルを呼び出すタイミングを理解するのに役立ちます。
3. 指示を読み、テストと lint のプロセスを導く方法を確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI から直接、または自然言語でスキルを呼び出せます。3 種類のチェックを実行するよう Copilot に依頼しましょう。

1. Interactive モードのフィルタリングの会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した作業と成功したかどうか、ツールからの実際の報告内容がわかるレポートに改善しましょう。スキルを更新して、その形式でレポートを作成します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、会話を再起動せずに編集済みスキルを再読み込みできる Copilot CLI で動作を確認しましょう。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果レポートの形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルタリングの機能 PR に含めます。次に、[Playwright MCP サーバーを使って][next-lesson] Copilot からサイトを直接操作します。

## スキルの例

コミュニティの次の例は参考情報であり、追加の作業ではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件文書: `prd`][prd-example]
- [図と同梱のエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
