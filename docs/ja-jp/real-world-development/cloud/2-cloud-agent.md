---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債には、多くの組織が悩まされています。未解決のセキュリティ問題、更新が必要なレガシー コード、実装の時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行します。作業を終えると、人間の開発者がレビューできる下書きの pull request (PR) を作成します。こうした退屈な作業を任せることで開発プロセスを速め、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って次のことを確認します。

- コードを生成するための環境をカスタマイズします。
- 操作を安全に実行する方法を確認します。
- Issue の範囲を明確にすることの重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys は技術的負債を抱えています。最初のサイトを作成した開発チームは、ドキュメントを十分に整備していません。まず、アプリケーションのエクスポートされた関数すべてに TSDoc ドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに、同じカテゴリに属する関連ゲームを表示すれば、利用者がほかのゲームも引き続き探せます。まだ完成度の高い実装は必要ありませんが、ユーザー エクスペリエンス (UX) の受け入れテストに使えるものを求めています。優先度がより高い Issue があるため、この作業は現在のブロッカーになっています。

どちらも優先度が下がりがちなタスクです。Copilot cloud agent に割り当てると、バックグラウンドで作業を進めている間、開発者は別の作業に取り組めます。その後、エージェントの作業をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じように、バックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると、Copilot は進行状況を追跡する下書きの pull request を作成し、環境を準備して、タスクに取り組みます。進行中または完了後にセッションを確認できます。提案された解決方法をレビューできる状態になると、pull request で開発者に通知します。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法のような機能はありません。考える作業を省略できるわけではありません。一見単純な操作にも、詳しく調べると複雑さがあります。Copilot cloud agent にタスクを依頼するときは、[範囲を慎重に定めてください][cloud-agent-best-practices]。AI ペア プログラマーのように扱い、段階的に進めて、学び、試し、調整します。生成 AI を追加しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示を扱う演習をすでに終えている場合は、`.github/copilot-instructions.md` がどのように使われるかを確認済みです。まだ確認していない場合は、この機会に見ておきましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を調べられるようにします。

GitHub の Web UI、または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメント コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターを確認します。これらの指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てます。生成された pull request に TSDoc ドキュメント コメント、TypeScript の規約、コメント ヘッダーが反映されているか確認してください。これらは指示ファイルに由来する内容なので、後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するには、通常、開発環境を準備し、必要なツールや依存関係をインストールする必要があります。Copilot にタスクを割り当てる場合も、同様のセットアップが必要です。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。作業を開始する前に実行する[専用のセットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、必要な開発ツールや依存関係を利用できるようになります。演習を進めやすくするため、このリポジトリではあらかじめ構成されています。単一の Astro アプリ向けに Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを実施済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。手順でクローンしない場合は、完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (現在、アプリ全体は Astro + Drizzle/libSQL で構成されています)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは通常の GitHub Actions ワークフローに似ていますが、重要な点がいくつかあります。

- `copilot-setup-steps` というジョブが1つあります。このジョブは、Copilot が pull request で作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使っているため、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が作業するのを待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、情報が古い、またはドキュメントがまったくないプロジェクトも多くあります。このような技術的負債を放置すると、生産性が下がり、コードベースの保守や新しい開発者の参加が難しくなります。Copilot はドキュメント作成を得意とするため、cloud agent に Issue を割り当てるのに適しています。cloud agent はバックグラウンドで必要なドキュメントを生成します。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。一覧に表示されない場合は、少し待ってブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot はここまでに説明したワークフローを実行して環境を作成し、プロジェクトを分析して、タスクへの取り組み方を決めます。

13. todo リストを確認し、Copilot が実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されない場合は、ウィンドウの再読み込みが必要になることがあります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて判断し、コードを作成する手順を観察できます。

完了まで数分かかることがあります。Copilot cloud agent の主な利点は、ほかの作業に集中できるよう非同期にタスクを進められることです。この機能を活用するため、もう1つタスクを割り当ててから、アプリに機能を追加する作業に移ります。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の1つは、作業を分担できることです。デザイン チーム向けの関連ゲーム表示は短時間で実装できるかもしれませんが、その間に別の作業を進められます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の Markdown を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、最初の Issue のコメントに 👀 が表示されます。これは Copilot が作業中であることを示します。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent は人間のソフトウェア エンジニアと同様に作業するため、常に監視する必要はありません。完了後に結果をレビューします。次はカスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に Issue を割り当てて非同期に作業させると、技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに活用できます。

次のことを行いました。

- コードを生成するための環境をカスタマイズしました。
- 操作を安全に実行する方法を確認しました。
- Issue の範囲を明確にすることの重要性を学びました。
- Issue を Copilot に割り当てました。

Copilot がバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用を確認します。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用できます][cloud-agent-mcp]。また、前のモジュールで扱ったカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
