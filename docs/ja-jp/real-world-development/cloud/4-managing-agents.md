---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの演習では、ユーザー エクスペリエンスの向上や機能の追加を目的に、Copilot cloud agent に3つの異なるタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの監視も重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがあります。この mission control では、未完了の pull request (PR) があるエージェント タスクを確認できます。実行した操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 実行中のセッションに追加の機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードを依頼した後、チームはライト モードも追加することにしました。サイトのスタイルと切り替え機能をすでに変更しているため、ライト モードも含めるのが合理的です。ハイ コントラストに加えてライト モードも実装するよう、作業中のエージェントに追加指示を送ります。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインに表示されるタスクの一覧を確認します。コードベースのドキュメント更新、ゲーム変更用 API の生成、Web サイトへのハイ コントラスト モード追加など、割り当てたタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が実施した作業を確認します。コードのチェックアウト、作業環境の作成、MCP サーバーのセットアップ、タスク完了に向けた操作などが表示されることがあります。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される pull request (PR) ペインも確認します。ここから PR と変更ファイルを表示して、作業状況をさらに確認できます。

## cloud agent に追加指示を送る

実行中のタスクを確認できたので、ハイ コントラスト モードの作業にライト モードも含めるよう依頼します。

1. ハイ コントラスト モードを追加しているセッションを選択します。正確なタイトルは、Copilot が付けた名前と作業状況によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで数分待ちます。以下のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェント タスクと steer active session while copilot is working ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に組み込むことを確認します。

## Copilot に作業を続けてもらう

前回と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送ったときに進めていた手順を終えると、新しい依頼を作業に反映します。

完了まで数分かかるため、その間にこれまでに学んだ内容を振り返ってもかまいません。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視してガイドする中心的な場所である Copilot agents ページを確認しました。この mission control で、進行中および完了したタスクを追跡し、作業内容をレビューし、実行中のタスクに追加の指示を送れます。

このレッスンでは、次のことを行いました。

- mission control と agents ページを使って cloud agent のタスクを監視しました。
- 実行中のセッションに追加の機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成した pull request を反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
