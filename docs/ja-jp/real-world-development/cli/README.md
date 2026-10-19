---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、pull request の自動化を学びながら、Tailspin Toys に2つの変更を加え、検証してリリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** を使うと、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できます。コードベースの調査、コードの生成、コマンドの実行、外部ツールへの接続をコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連のワークフローを進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールしてプロジェクトを確認します。
2. 星評価に対象を絞った変更を加え、ブラウザーでレビューして、最初の pull request (PR) を手動でマージします。
3. フィルター Issue から始めて、Plan モードで実装方針を決め、Autopilot モードで構築し、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能に適用します。
5. 既存の `quality-checks` skill をカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加して、ブラウザーでフィルター機能を確認します。
7. 品質保証 (QA) カスタムエージェントを作成し、要件、テスト カバレッジ、検証の根拠をレビューします。
8. フィルター機能の変更全体を確認し、Agent Merge を使って PR をマージします。
9. コンテキスト、モデル、共有、省略可能なクラウド委任に使える slash command を確認します。

ワークショップの焦点を保つため、星評価の PR と、指示の更新・skill の更新・QA プロファイル・テストを含むフィルター機能の PR の2つを作成します。フィルター機能と品質ワークフローは1つの会話とブランチで進めるため、各ツールを試しながら、それまでの作業を活用できます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、プロジェクトを確認します |
| [2. 星評価を追加して小さな成果を得る][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加して、最初の PR をマージします |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用します |
| [5. quality-checks skill をカスタマイズして使用する][ex5] | 繰り返し実行できるチェック | 既存の skill を確認し、レポート形式を変更して実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を設定して、フィルターの動作を確認します |
| [7. QA エージェントを作成して使用する][ex7] | 要件とカバレッジ | 専門プロファイルを作成して選択し、最終検証の根拠を集めます |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューして PR を作成し、Agent Merge を使います |
| [9. GitHub Copilot CLI の slash command][ex9] | CLI の機能 | コンテキスト、モデル、共有、省略可能な cloud agent への委任を確認します |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加リソースを振り返ります |
| [省略可能: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイして Web サイトに接続します |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランを利用していませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
