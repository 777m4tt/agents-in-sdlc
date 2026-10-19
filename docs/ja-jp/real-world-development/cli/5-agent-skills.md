---
title: "レッスン 5 - quality-checks skill をカスタマイズして使用する"
description: "既存の quality-checks skill を確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。コードを手動で検証し、指示ファイルを使って標準に従うようにしました。次はテストや lint など、継続的インテグレーション (CI) に関わる作業も確認します。

このようなタスクには **agent skill** が適しています。skill を使うと、これらの操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果の形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニット テスト、lint、型チェックに `quality-checks` skill を使用しています。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをパッケージ化し、必要に応じてエージェントが読み込みます。基本的には、skill の名前を付けたフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルには、skill の名前と説明を指定するフロントマター、処理の概要、呼び出すタイミングに関するガイダンスを記述します。フォルダーには、skill が使うスクリプトやその他のリソース用のサブフォルダーも含められます。

> [!NOTE]
> skill に追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトにすでにあるコマンドを使うため、`SKILL.md` だけで構成されています。

skill は、チームで共有して再利用できるプロジェクトの `.github/skills` フォルダーか、ユーザーの skill フォルダー `~/.copilot/skills` に配置できます。

## skill を確認する

Tailspin Toys チームがユニット テスト、lint、型チェックのために作成した `quality-checks` skill を確認します。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明は、Copilot が skill を呼び出すタイミングの判断に役立ちます。
3. 指示を読み、テストや lint のプロセスをどのように案内しているか確認します。
4. skill に **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

skill を変更する前に、Copilot に3つのチェックを実行するよう依頼します。

1. Interactive モードでフィルターの会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行された内容、成功・失敗の状態、ツールが報告した結果が分かるレポートを作成するよう、skill を更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新した skill を実行する

変更した skill が結果にどう反映されるか確認します。Copilot CLI は会話を再起動しなくても、編集した skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と同じプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初の結果と比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニット テスト、lint、型チェックを行う `quality-checks` skill を確認しました。
- 結果レポートの形式をカスタマイズしました。
- skill を再読み込みして実行しました。

この変更は、フィルター機能の PR に含まれます。次に、[Playwright MCP サーバーを使って Copilot がサイトを直接操作できるようにします][next-lesson]。

## skill のその他の例

以下のコミュニティの例は参考資料であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [貢献ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート用スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
