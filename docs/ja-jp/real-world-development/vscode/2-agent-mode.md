---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの小さな更新でも、複数のファイルを変更し、テストを実行する必要があることがよくあります。通常、開発者は必要なファイルを探し、変更を加えて、テストを実行します。問題を修正し、見落としたファイルがあればさらに更新します。こうした作業は際限なく続くことがあります。

そのような作業に役立つのが Copilot エージェント モードです。

Copilot エージェント モードは、統合開発環境 (IDE) でより自律的に動作します。開発者のように既存のプロジェクト構造を調べ、必要な更新を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェント モードでサイトに機能を追加する方法を確認します。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる体験を提供します。エージェント モードは IDE で作業し、すばやく対話しながら反復します。Cloud agent はチームの同僚のように非同期で作業し、Issue と pull request を通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot エージェント モードでプロジェクトを調査し、関連ファイルを特定して、複数の変更を連携して行う。
- UI とデータ層にまたがる新機能を実装する。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行者で絞り込めるようにします。前の演習で発行者ヘルパーを追加したので、次は Copilot エージェント モードを使って、残りのデータ層、ユーザー インターフェイス (UI)、テストを完成させます。

## Tailspin Toys Web サイトを実行する

変更を加える前に、現在の機能を把握するため Tailspin Toys Web サイトを確認します。

この Web サイトは、開発者をテーマにしたボード ゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは Drizzle ORM を通じて、フロントマターからローカルの SQLite データベースに直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、対話機能は標準の DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に起動できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習を終えていれば `main` ブランチにいます。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くためのダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナル内の Astro サーバー アドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> codespace でターミナルの localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは、Web サーバーを実行している codespace へのプライベート トンネルです。

### Web サイトを確認する

サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **Home Page**: ボード ゲームの一覧、タイトル、画像、説明を表示します。
- **Game Details Page**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの情報が記載された詳細ページを表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は、手順の中で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まず、バックログを確認します。テンプレートからプロジェクトをセットアップしたときに Issue のバックログも自動的に作成されました。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** はファイルをまたいだ変更を自律的に計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時には Claude Sonnet 4.5 を使用したため、手順に含めています。提案の一貫性を保ち、スムーズな体験にすることを目的としています。ただし、大規模言語モデル (LLM) は確率的に動作するため、ワークショップの例と異なる提案が表示される場合があります。これは正常で想定された動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が利用者に最も役立つと思われる項目を優先順位付けしたことを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える指示ファイルを確認します。[ユーザー インターフェイス (UI)][ui-instructions]の指示ファイルには、Web サイトの機能追加に取り組む方法が記載されています。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの指示へのリンク

> [!TIP]
> 指示ファイルからほかの指示ファイルやプロジェクト ファイルを参照できます。パスは指示ファイルからの相対パスです。これにより、指示の再利用、複雑な指示の小さな単位への分割、例やテンプレートの提示ができます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## フィルター機能を実装する

フィルター機能を完成するには、アプリケーションに少なくとも3つの更新が必要です。

- データ層 (`src/lib/`) に残りのフィルター ロジックを追加または調整する
- フィルターの動作に関するテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

また、コードベースにマージする前にテストを実行し、成功することを確認します。Copilot エージェント モードにこれらの作業を依頼します。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを見つける様子を確認します。データ層ヘルパー、UI、テストを特定してファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行など、複数の作業を行います。タスクやコードベースの複雑さによっては数分かかることがあります。作業中にエディターで **Keep** と **Undo** が表示される場合があります。完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。作業の途中で選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![テスト実行の確認を求める Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストに失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが検出されなくなるまで作業します。

   ![Copilot エージェント モードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに潜在的な問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェント モードを使って Tailspin Toys の Web サイトに機能を追加しました。次のことを行いました。

- Copilot エージェント モードでプロジェクトを調査し、関連ファイルを特定して、複数の変更を連携して行いました。
- UI とデータ層にまたがる新機能を実装しました。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューしました。

次に、[Playwright MCP サーバーで機能をテストし][next-lesson]、pull request を作成します。

### ボーナスの探索演習 — ページングを実装する

ゲームの一覧が増えると、ページングが必要になります。この演習で学んだ内容を使い、Copilot にページングの実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従う。
- ページ サイズをユーザーに選択させるか、固定するかなど、ページングの実装方法を決める。
- 希望する実装に必要なガイダンスをプロンプトに含める。
- Copilot に変更を依頼したりコンテキストを追加したりしながら反復する。Copilot と作業するときは通常このように進めます。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
