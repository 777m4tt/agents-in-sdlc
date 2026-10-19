---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を確認し、Autopilot で Issue に基づくフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに合わせて使えるため、適切な方法で適切な機能を構築できます。このレッスンから、Issue を使って機能を作成し、コードを検証し、動作を確認して、最終的にプロジェクトにマージする、エージェントを活用した開発プロセスを進めます。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、このワークショップでは中核となる概念に集中するため、手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードを使って機能を実装します。
- コードをレビューします。
- ポート転送されたブラウザーで機能を手動で検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` skill をカスタマイズし、MCP による検証を追加して、QA エージェントを作成し、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えてきたため、利用者がカテゴリや発行者でゲームを絞り込めるようにします。バックログの Issue には機能が記載されていますが、複数カテゴリの組み合わせなど、コーディング前に合意すべき詳細があります。Plan モードで決定事項を整理し、範囲を定めてから Autopilot に実装を許可します。

## 背景

AI コーディング エージェントを開発フローに導入しても、ソフトウェア開発の基本は変わりません。むしろ、その重要性は増します。多くの開発者は、次のような流れで作業します。

1. 実装する内容を記述した Issue から始めます。
2. 構築するものの計画を立てます。
3. コードを構築してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功した後、変更をマージします。

> [!NOTE]
> 正確な手順はチームや組織によって異なりますが、多くの場合、ここに示した流れを基本とします。

標準的な方法に沿って進めると、AI が生成したコードが要件を満たし、手書きのコードと同じ検証プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどれだけの自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: 開発者とエージェントが協力して作業します。エージェントは変更を提案し、続行する前に入力を待ちます。
- **Plan**: エージェントは最初に計画を作成し、プロジェクト ファイルの編集を行いません。
- **Autopilot**: エージェントは自律的にコードを書き、テストを実行し、タスクが完了したと判断するまで反復します。

最初に Plan モードで計画を確認し、次に Autopilot で実装します。

## Issue から始める

フィルター機能に取り組む前に Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動してから続行します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7`（または同様の番号）で始まることを確認します。`#` を使うと GitHub の Issue または pull request (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に実装を任せる前に、機能の実装方法やタスクの進め方を決められます。複雑な変更に取り組むときは、時間をかけて計画することをお勧めします。Plan モードに切り替え、計画を作成するよう Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 直前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、適切な実装方法を判断し、計画を作成します。

3. フィルター機能の動作について Copilot から質問された場合は、自分の希望に沿って回答します。ここには唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 自分の決定と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたので、次は実装します。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot がタスクの完了を判断するまで問題に取り組み、反復します。

1. **Accept plan and build on autopilot (recommended)** を選択します（利用中のバージョンで同様のラベルが表示される場合は、その選択肢を使います）。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行しながら反復する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。先に計画を確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェックが完了したと判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様に、マージする前にレビューします。コードを確認し、サイトを起動して動作も検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画時の決定に照らして確認します。
4. コードのレビューが終わったら <kbd>Esc</kbd> を押して diff 表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードを確認したら、アプリを起動して、Codespaces が転送したポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. カテゴリによる絞り込み、発行者による絞り込み、計画で合意した組み合わせを試します。
4. リセットと検索結果がない場合の動作が、Issue と合意した内容に沿っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードを使って機能を実装しました。
- コードをレビューしました。
- ポート転送されたブラウザーで機能を手動で検証しました。

次に、[カスタム指示を使ってコード生成が文書化された慣行に従うようにします][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
