---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を、反復しながら進めます。

## シナリオ

ボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に新しく加わった開発者として作業します。チームのバックログは GitHub issue として登録済みで、すぐに取りかかれます。フィルタリングやページネーションなどの機能追加に加え、アクセシビリティやコーディング標準などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、どこで作業していても利用できます。開発方法に合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内し、進行状況を確認して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。Copilot Chat の agent mode、MCP サーバー、カスタムエージェントをエディターから離れずに活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
