---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、Copilot を特定の作業に導く指示、ベスト プラクティス、コーディング パターン、分野固有の知識を指定します。チームの専門知識を再利用可能なエージェントにまとめると、[Web Content Accessibility Guidelines (WCAG)][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディングを実践するセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどを作成できます。

カスタムエージェントはプロジェクトの `.github/agents` フォルダー、またはユーザー全体に適用する `~/.copilot/agents` に Markdown ファイルとして定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントと agent skill の違い

カスタムエージェントと[agent skill][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。簡潔に言うと、**カスタムエージェントは作業者、skill はツール**です。

カスタムエージェントには独自のコンテキスト ウィンドウがあり、作業の一環として skill やほかのエージェントを調整できます。この演習では、アクセシビリティ カスタムエージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。作業中に、pull request ワークフローの skill やテストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの利用方法と同様、環境やシナリオに合わせてテストと改善を重ねてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダーク テーマを読みづらいと感じるユーザーがいることが分かりました。このアクセシビリティの問題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を求めています。

アクセシビリティは重要であるため、できるだけ早く実装します。機能の生成にはカスタムエージェントを使います。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使って、ハイ コントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに作成されています。Copilot をどのようにガイドするのか理解するため、内容を確認します。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します（演習 3 のフィルター PR とは分けます）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコードを生成するときの主な責務、アクセシビリティのベスト プラクティス、HyperText Markup Language (HTML)・Cascading Style Sheets (CSS)・JavaScript のコード例、よくある問題が記載されています。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義したすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、セッションでそのエージェントの指示とツールを使えます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は、手順の中で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていない場合は、選択します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。選択中のエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集します。各編集にはファイル パスと diff が表示されます。
6. 編集が行われると、**Files changed** の表示が更新され、エージェントが変更した作業セットを確認できます。

> [!NOTE]
> この処理には数分かかることがあります。Copilot はリポジトリを実際に変更します。作業中に Astro コンポーネント、CSS、関連テストなど既存ファイルを編集する場合があります。

次の演習で、この作業中のセッションをレビューして方向付けます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの専門知識と標準を再利用可能な形にまとめ、Copilot をガイドできます。

このレッスンでは、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat のエージェント モードでカスタムエージェントを使いました。

次に、エージェントの作業を[監視して方向付けます][next-lesson]。変更を確認しながら、同じセッションにライト モードの切り替え機能を追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
