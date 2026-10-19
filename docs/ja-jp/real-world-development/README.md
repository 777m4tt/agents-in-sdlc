---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、pull request のレビュー、エージェントを使った作業管理を反復して行います。

## シナリオ

架空の企業 Tailspin Toys の新しい開発者として作業します。同社はボード ゲームを対象とするクラウドファンディング サービスを提供しています。チームのバックログは GitHub Issue として登録済みです。フィルターやページネーションなどの機能追加に加え、アクセシビリティやコーディング標準などの品質改善にも取り組みます。

## 作業環境を選択する

GitHub Copilot は、開発者の作業環境に合わせて利用できます。希望する開発方法に合った環境を選択してください。各ワークショップでは、それぞれセットアップを行い、Tailspin Toys の共通シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、skill、カスタムエージェント、Playwright Model Context Protocol (MCP)、Agent Merge、実用的な slash command を扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並列に実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue と pull request を管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業をガイドして進行状況を監視し、エージェントが作成した pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat のエージェント モード、MCP サーバー、カスタムエージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
