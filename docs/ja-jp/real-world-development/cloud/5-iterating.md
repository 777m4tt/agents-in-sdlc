---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このハーネスでは、ユーザー エクスペリエンスの向上や機能追加を目的として、GitHub Copilot にいくつかのタスクを依頼しました。コードへのドキュメント追加、デザイン チームが反復して改善できる関連ゲーム機能の構築、ハイ コントラスト モードとライト モードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善します。

### シナリオ

これまで何度も確認したように、生成 AI を導入してもソフトウェア設計や DevOps の基本は変わりません。生成コードをレビューし、通常の DevOps プロセスに沿って作業する必要があります。チームのほかのメンバーにレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期で監督なしにタスクを実行するため、安全に作業できるよう、次のセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスはコードを作成するブランチに**限定**されます。
- cloud agent は GitHub Actions 内で、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。Model Context Protocol (MCP) サーバーも対象です。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した pull request (PR) のうち、コードへのドキュメント追加を確認します。GitHub.com の通常の PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。既定では外部リソースへのアクセス（外部 MCP サーバーへの呼び出しなど）が制限されているため、これは**想定される状態**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** のようなタイトル（または、より説明的なタイトル）の PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は作業が完了するまで待ちます。数分かかることがあるため、休憩したり、ここまでに学んだ内容を振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択して変更を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc ドキュメント コメントなど、新たに追加されたドキュメントを確認します。実際の変更内容は異なる場合があります。

    変更を確認するときは、TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーに注目します。これらは演習 2 の最初に確認したカスタム指示ファイルに由来します。

6. 更新内容を確認したら、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

pull request 上で Copilot とやり取りする方法は一方通行ではありません。チーム メンバーと同じように、PR のコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。生成結果は確率的なため、固定のプロンプトは指定できません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、そのファイルの概要を示すコメント ヘッダーを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc ドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例、またはコードベースに追加したいドキュメントの要望を使い、**@copilot** にメンションして変更を依頼するコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新のために新しいセッションが開始されます。
3. **Back to pull request** を選択すると PR に戻れます。

    ![pull request に戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復して改善できます。準備ができたら、PR を下書きからレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** のようなタイトル（または、より説明的なタイトル）の PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容を確認したら、**Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` を Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの件数を調整したり、コメント ヘッダーと TSDoc ドキュメント コメントを追加したりできます（この Issue はカスタム指示の更新**前**に割り当てたことを思い出してください）。前の PR と同様に、**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタムエージェントを使って実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** のようなタイトル（または、より説明的なタイトル）の PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装をレビューし、特に次の点を確認します。
   - モードを切り替えるユーザー インターフェイス (UI) コンポーネント
   - ユーザーの設定を保持する local storage の使い方
   - ハイ コントラスト モードとライト モードの Cascading Style Sheets (CSS) またはスタイル変更
   - アクセシビリティ属性（Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など）
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認したら、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します（`<branch-name>` を Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリを起動し、ブラウザーでハイ コントラスト モードとライト モードの切り替えをテストします。ページを再読み込みした後も設定が保持されることを確認します。

アクセシビリティ カスタムエージェントが、アクセシビリティのベスト プラクティスに沿った機能実装を Copilot にガイドしたことを確認します。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に修正を依頼できます。

## 省略可能な演習 — 委任を続ける

バックログの実際のタスクをエージェントに任せ、その間に別の作業を進める方法を練習します。リポジトリに Issue をいくつか追加して Copilot に割り当てます。たとえば、次のようなタスクがあります。

- ゲーム詳細ページに支援者の関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。ここまでのレッスンでは、次のことを行いました。

- cloud agent の出力への影響を後で確認するため、このリポジトリに含まれるカスタム指示ファイルを調べました。
- Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期での作業を確認しました。
- アクセシビリティ向けカスタムエージェントを作成して使い、ハイ コントラスト モードとライト モードを追加しました。
- Copilot agents ページを mission control として使い、進行中のアクセシビリティ セッションを監視して方向付けました。
- Copilot の pull request をレビューして反復し、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。ほかのハーネスで、ここで学んだ内容をさらに確認できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP の統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、agent skill、カスタム QA エージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` による cloud agent への連携を使って、レビュー済みの2つの変更をターミナルからリリースします。

自分のリポジトリで、次のアイデアを試してみてください。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 別の分野に対応するカスタムエージェントを作成します。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
