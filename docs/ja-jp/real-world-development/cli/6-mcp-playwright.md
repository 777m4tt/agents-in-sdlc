---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加することも必要です。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能を超えるツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を確認します。
- Playwright MCP サーバーがまだない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニット テストやエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。ユーザーと同じように Copilot に Web サイトを操作させることで、変更が想定どおりに動作するかをさらに自動的に確認できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信する方法を提供します。MCP により、エージェントは最新情報にアクセスし、開発者に代わって操作を実行できます。

ツールやリソースには MCP サーバーを通じてアクセスします。MCP サーバーは AI エージェントと外部ツールやサービスの橋渡しをします。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーには次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、pull request を管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、貢献を促すための [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーも、プロジェクトのほかの依存関係と同じように扱ってください。利用する前にソース コードを確認し、公開元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、セットアップを省略して次のセクションに進みます。

1. codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力して <kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類を **STDIO** にし、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って機能を確認するよう Copilot に依頼する

これまでは機能が想定どおりに動作するかを手動で確認しました。次は Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、MCP サーバーを自動で判断するため、明示的に指定する必要はありません。ただし、使用するサーバーが分かっている場合は、プロンプトで示すことで、より一貫した結果を得やすくなり、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために実行する手順を観察します。
3. レポートを読み、すべてが想定どおりに動作するか確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から実際のブラウザーで機能を確認するため、Playwright MCP サーバーを使いました。次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での利用方法を確認しました。
- Playwright MCP サーバーがまだない場合は追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次に、skill とブラウザー ツールを専門的な役割にまとめる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
