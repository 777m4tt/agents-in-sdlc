---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローを進めました。次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを確認して、用意されているフィルター Issue を見つけました。
- 星評価を追加し、ポート転送されたブラウザーで結果を確認して、最初の pull request (PR) を手動でマージしました。
- フィルター Issue から始めて Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` skill をカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、skill の結果、ブラウザーで確認した根拠を評価しました。
- フィルター機能の変更全体をレビューし、Agent Merge を許可してフィルター機能の PR をマージしました。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使う slash command を確認しました。

## リリースした内容

このワークショップでは、更新済みの `main` から作成したブランチで、2つの PR を進めました。

1. **星評価**: ゲーム カードに既存の `starRating` と、評価がない場合の表示を追加します。
2. **フィルター機能と品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連テストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。この作業を1つの PR にまとめ、ワークショップの手順を簡潔にしました。

## 異なる検証方法

自動テスト、自分で行うブラウザー確認、MCP を通じて Copilot が行うブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` skill は、ユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は、PR の前にこれらの結果と、要件やテスト カバレッジをまとめて確認しました。

追加するテストでは、実際の不足を補います。QA の実行で新しいテストが必要ない場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける根拠を再確認してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、skill をカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、skill は繰り返し実行できるタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選択します。実装前に方針を検討するには **Plan**、範囲を絞った変更に関わり続けるには **Interactive**、範囲が明確な作業には **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を、なぜ、どのように構築するかを明確に説明すると、結果が大きく変わります。

## さらに試す

基本的なワークフローを確認しました。Copilot CLI で試せる機能をいくつか紹介します。

- `/review` を使って、コードレビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` を使って問題を説明し、別の視点から考えます。
- `/fleet` を使って、独立したサブタスクを並列に実行します。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、タスクを Copilot cloud agent に送ります。

## 次のステップ

ツールの使い方を上達させるには、実際に使い続けることが大切です。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなどで試してください。学んだことをチームで共有し、ほかの開発者の経験から学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
