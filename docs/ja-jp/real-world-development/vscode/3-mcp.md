---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェント モードを使ってフィルター機能を構築しました。pull request を開く前に、ブラウザーで実際に動作することを確認します。アプリを自分で操作する代わりに **Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にブラウザーでフィルター機能を手動でテストするよう依頼します。
- ブランチを公開し、フィルター作業の pull request を開きます。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外部にアクセスできるとさらに便利になります。Model Context Protocol (MCP) は、Copilot エージェントが外部のツールやサービスと通信するための標準です。

![エージェント モードの仕組みと、コンテキスト、大規模言語モデル、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、人工知能 (AI) エージェントが外部のツールやサービスとリアルタイムに通信できます。リソースを使って最新の情報にアクセスし、ツールを使って開発者の代わりに操作を実行できます。

ツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部のツールやサービス（既存のアプリケーション プログラミング インターフェイス (API) や npm パッケージなどのローカル ツール）との通信を管理します。各 MCP サーバーがエージェントに提供するツールやリソースは異なります。

よく使われる MCP サーバーには次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。リポジトリの作成や更新、Issue と pull request の管理などを AI エージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォーム入力、ボタンの選択などを AI エージェントが実行できます。

ほかにも、さまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステム内での MCP サーバーの発見と貢献を促すため、[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティに関して、MCP サーバーはプロジェクトのほかの依存関係と同じように扱ってください。利用前にソース コードを注意深くレビューし、公開元を確認して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密性の高いリソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` 設定はプロジェクト テンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールするものはありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供するため、機能のテストに使えます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` を追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグを指定すると、表示ウィンドウなしでブラウザーを実行します。デスクトップのない codespace では、この指定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではすでにエンドツーエンド テストに Playwright を使っているため、ブラウザーは通常インストール済みです。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot が初めてサーバーのツールを使うときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** アクションが表示されます。これを選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認し、信頼することを選択して起動を許可します。

手動での起動は省略できます。その場合、次の手順で Copilot がツールを初めて使うときに、VS Code がサーバーを起動して信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。アプリを自分で操作する代わりに、エージェントがページを開いて移動し、フィルターを適用して結果を確認し、内容を要約します。会話を離れずに機能が想定どおりに動くかを確認する最も速い方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じように、ラベル付きの構造化要素（ボタン、リンク、リスト項目など）を使って判断します。そのため、簡単な機能確認がアクセシビリティの基本的なチェックにもなります。

サーバーが接続され、アプリが実行中の状態で、Copilot に次のプロンプトを送信し、フィルター機能を試すよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件に照らして要約を確認します。問題があれば、追加で質問するか、pull request を開く前にコードを修正するよう依頼します。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止している場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーをダウンロードすることがあります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認したので、チームがレビューできるよう pull request (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## pull request を作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、pull request の作成方法はいくつかあります。今回は GitHub Copilot に PR を作成してもらいます。関連する Issue を見つけ、PR を Issue に関連付けられます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** が選択されていることを確認します。これにより、Copilot は GitHub ツールを使用できます。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の収集や操作を Copilot に許可するため、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときに、GitHub で認証を求められる場合があります。指示に従って許可します。
5. Copilot が Issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP サーバーを使って、リリース前にブラウザーでフィルター機能を手動でテストしました。
- ブランチを公開し、フィルター作業の pull request を開きました。

機能のテストに MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の1つにすぎません。次に、アクセシビリティ レビューなどの対象を絞ったタスクを効率化するため、[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
