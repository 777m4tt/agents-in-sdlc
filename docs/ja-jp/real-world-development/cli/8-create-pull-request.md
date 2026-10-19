---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、skill の更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、指示の更新、skill の更新、品質保証 (QA) プロファイル、テストは、1つのブランチに保存されています。これらをまとめてレビューし、pull request を作成します。星評価の pull request (PR) は手動でマージしました。今回は **Agent Merge** にマージの処理を任せます。

> [!NOTE]
> 通常は機能、指示の更新、skill の更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルターと品質ワークフローに関する作業を1つの会話とブランチにまとめ、1つの PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージまでのライフサイクルを自動化する仕組みを確認します。
- 機能変更全体と検証の根拠を確認します。
- フィルター機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフローでは、Copilot を使って機能を計画、実装、検証しました。Tailspin Toys では、マージを許可するかどうかは開発者が決定したうえで、PR に関する残りの作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、pull request をマージ可能な状態にするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題に取り組みます。たとえば、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースを行います。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にその処理を任せることもできますが、有効にする前に作業をレビューし、マージ可能であると判断してください。

## Agent Merge を使って PR を管理する

コードをすべて作成したので、まとめてレビューし、PR を作成して、残りの処理を Agent Merge に任せます。

1. codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って、新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべてのチェックが成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている方法でレビュー済みの PR を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、pull request の処理など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージまでのライフサイクルを自動化する仕組みを確認しました。
- 機能変更全体と検証の根拠を確認しました。
- フィルター機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次に、コンテキスト、モデル、共有、cloud agent への省略可能な委任に使える[Copilot CLI の slash command を確認します][next-lesson]。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
