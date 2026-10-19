---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks skill、Playwright MCP を活用する QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` skill を使って自動チェックを実行し、Playwright MCP を使ってブラウザーでフィルター機能を確認しました。次に、明確に定義された QA プロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、skill、MCP ツールを組み合わせる仕組みを確認します。
- 再利用可能な品質保証 (QA) プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、pull request (PR) を開く前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントを使うと、QA プロセスを調整して再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルでは、エージェントの目的、指示、使用できるツールを指定します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` skill は繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、それらを要件の評価や結果の報告にどう使うかを Copilot に伝えます。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能 PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。このプロファイルでは、QA が行うチェックと守るべき制約の両方を定義します。

1. codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと制約をプロファイルに反映したか確認します。これにより、検証だけを求めているときに、不完全なエージェントや範囲の広すぎるエージェントが機能を変更することを防ぎます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略できますが、指定すると一覧で分かりやすい表示名になります。
3. プロファイルの指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` skill を実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を報告し、実装コードを変更する前に確認を求め、変更をコミットせず pull request も開かないことを確認します。
5. これらの責務や制約が欠けている場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue を使って QA を実行する

Copilot CLI は、会話を開始したときにプロジェクトのエージェントを読み込みます。プロファイルの作成後、同じフィルターの会話を再開して QA を選択すると、それまでの Issue や計画の決定を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、利用可能なエージェント一覧にまだ表示されない場合があります。次のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに追加し、その結果をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、skill、MCP ツールと連携する仕組みを確認しました。
- 再利用可能な QA プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター Issue に照らして結果をレビューしました。

実装、skill の更新、QA プロファイル、テスト、検証レポートがそろいました。次に、[機能 PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
