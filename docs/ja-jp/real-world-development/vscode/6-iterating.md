---
title: "演習 6 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
| :-- |

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの向上を目的とするいくつかのタスクに GitHub Copilot と取り組みました。エージェント モードを使ってクライアントとサーバーにフィルター機能を追加し、Playwright MCP サーバーを使ってブラウザーで手動テストしました。その後、カスタムエージェントを使って、ハイ コントラスト モードとライト モードの切り替えを含むアクセシビリティ機能を実装し、作業中のセッションに追加指示を送って機能を拡張しました。次に、ローカルの作業を公開し、チームと同じ方法でレビューします。

### シナリオ

生成 AI を導入しても、ソフトウェア設計や DevOps の基本は変わりません。Copilot が作成した内容も、通常のレビュー サイクルに通す必要があります。codespace からアクセシビリティの変更をプッシュして pull request を開き、チームのほかのメンバーを加える前に差分を確認します。

## アクセシビリティ機能を公開する

[演習 4][exercise-4]と[演習 5][exercise-5]でアクセシビリティ カスタムエージェントを使って実装したハイ コントラストとライト モードのトグルは、コミット済みの変更として codespace にあります。ブランチをプッシュして pull request を開き、チームがレビューできるようにします。

1. codespace に戻ります。
2. VS Code で **Source Control** ビューを開きます。
3. アクセシビリティの変更がコミット済みであることを確認します。演習 5 の未コミットの変更がある場合は、ステージして、`Add high-contrast and light-mode toggles` など内容が分かるメッセージでコミットします。
4. **Publish Branch** を選択してブランチを公開します（または **...** メニュー → **Push** を選択します）。
5. VS Code から github.com で新しいブランチを開くように求められたら、プロンプトを受け入れます。またはリポジトリに移動し、ブランチ バナーで **Compare & pull request** を選択します。
6. 分かりやすいタイトル（例: `Add high-contrast and light-mode toggles`）を設定し、何をなぜ行ったかを簡潔に説明します。
7. **Create pull request** を選択します。
8. PR が開いたら **Files changed** タブを選択し、作業全体をレビューします。特に次の点を確認します。
   - モードを切り替えるトグルのユーザー インターフェイス (UI) コンポーネント
   - ユーザー設定を保持する local storage の使い方
   - ハイ コントラストとライト モードの Cascading Style Sheets (CSS) またはスタイルの変更
   - アクセシビリティ属性（Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など）
   - モードの切り替えを管理する JavaScript/TypeScript コード
9. **Conversation** タブに戻ります。
10. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. ワークフローが完了するまで待ちます。問題がなければ、成功したことを確認できます。

> [!TIP]
> アクセシビリティの作業について別の意見が必要ですか。PR コメントで `@copilot` にメンションし、「この PR にほかの WCAG の問題がないかレビューしてください」や「キーボード操作の改善を提案してください」などと依頼します。Copilot はコメントに対応する新しいセッションを開始します。

## 省略可能な演習 — ローカルでさらに試す

IDE でエージェントと反復して作業するスキルは、繰り返し練習することで身につきます。VS Code から開始できる追加セッションの例を紹介します。

- ゲーム詳細ページに支援者の関心フォームを追加します。
- ゲーム一覧ページにページネーションを実装します。
- `src/lib/` のデータ アクセス ヘルパーに入力検証とエラー処理を追加します。
- アクセシビリティ エージェントの範囲を広げます。たとえば、サイト全体のキーボード フォーカス順序を監査します。

## まとめ

おめでとうございます。VS Code ハーネスを完了しました。このラボでは、次のことを行いました。

- **Playwright MCP を使って機能を手動でテストしました。** Playwright MCP サーバーを追加し、pull request を開く前に Copilot にブラウザーを操作させてフィルター機能を検証しました。
- **エージェント モードでスタック全体にまたがる変更を連携して進めました。** 1つのセッションで、クライアント、サーバー、テストにまたがるフィルター機能を追加しました。
- **カスタムエージェントを使いました。** エージェント選択メニューでアクセシビリティに特化したカスタムエージェントを選び、リポジトリのハイ コントラスト モードを実装する様子を確認しました。
- **エージェント セッションを管理して方向付けました。** 提案された変更をインラインでレビューして必要なものを承認し、ライト モードの追加を依頼してセッションを拡張しました。
- **pull request で作業を完了しました。** ローカルの作業を公開して、チームと同じように全体をレビューしました。

## レビューと次のステップ

必須の VS Code ハーネスを完了しました。ここでワークショップを終了できます。

Copilot のエージェント機能をさらに確認する場合は、別の環境を使う関連シナリオをほかのハーネスで体験できます。

- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、skill、カスタム QA エージェント、Playwright MCP、Agent Merge、コンテキストを考慮した引き継ぎを使い、ターミナルからレビュー済みの変更を2つリリースします。
- ☁️ **[Cloud agent ハーネス](../../cloud/)** — cloud agent への Issue 割り当て、agents ページでのセッション監視、pull request での非同期の反復作業を扱います。

ここで始めた作業をさらに進めることもできます。[awesome-copilot][awesome-copilot] には、自分のプロジェクトに応用できる指示ファイル、カスタムエージェント、skill が豊富にあります。

省略可能な拡張として、[省略可能: Foundry を組み込む][exercise-7]では、VS Code と Microsoft Foundry Toolkit を使ってモデルを準備し、Backer Concierge をデプロイしてサイトに接続します。

## リソース

- [GitHub Copilot][github-copilot]
- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェント モードを使う][agent-mode]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
