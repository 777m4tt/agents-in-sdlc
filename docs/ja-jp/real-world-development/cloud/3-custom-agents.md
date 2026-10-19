---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、エージェントの動作、専門知識、指示を定義します。焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識により、特定の作業をより効果的に進められます。チームの知識を再利用可能なエージェントにまとめることで、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを実践するセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントなどを作成できます。

カスタムエージェントはプロジェクトの `.github/agents` フォルダー、またはユーザー全体に適用する `~/.copilot/agents` に Markdown ファイルとして定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントと agent skill の違い

カスタムエージェントと[agent skill][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。簡潔に言うと、**カスタムエージェントは作業者、skill はツール**です。

カスタムエージェントには独自のコンテキスト ウィンドウがあり、作業の一環として skill やほかのエージェントを調整できます。この演習のアクセシビリティ カスタムエージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし更新します。その作業の中で、pull request ワークフローの skill やテストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI のほかの利用方法と同様、環境やシナリオに合わせてテストと改善を重ねてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次のことを行います。

- カスタムエージェントの定義方法を確認します。
- タスクをカスタムエージェントに割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが不十分なため、現在のダーク テーマを読みづらいと感じる人がいることが分かりました。このアクセシビリティ上の問題に対処するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。機能の生成にはカスタムエージェントを利用します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはあらかじめ作成されています。Copilot をどのようにガイドするのか理解するため、内容を確認します。

codespace に戻り、次のファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコードを生成するときの主な責務、アクセシビリティのベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題が記載されています。

## Issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、作業中のセッションに追加の指示を送れます。まず、ハイ コントラスト モードの作成タスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![cloud agent の割り当て画面でカスタムエージェントと Accessibility が強調表示されたスクリーンショット](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]を確認しました。カスタムエージェントを使うと、チームの知識や標準を再利用可能な形にまとめ、Copilot がより効果的に作業できるようガイドできます。

このレッスンでは、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- タスクをカスタムエージェントに割り当てました。

Copilot がハイ コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
