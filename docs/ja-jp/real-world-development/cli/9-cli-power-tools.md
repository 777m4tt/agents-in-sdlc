---
title: "レッスン 9 - GitHub Copilot CLI の slash command"
description: "コンテキスト管理、モデルの選択、セッションの共有、並列作業やクラウドへの委任に使う省略可能なコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの CLI ツールと同様、GitHub Copilot CLI にも対話に使える slash command が多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認します。

## シナリオ

CLI の基本的なワークフローが完了しました。次に、コンテキストの管理、モデルの切り替え、セッションの共有、省略可能な [Copilot cloud agent][about-cloud-agent] への委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、slash command を使って状態を調べたり、手動で圧縮したりできます。

1. codespace に戻ります。Copilot CLI がまだ開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、コンテキストがシステム指示、ツール、メッセージ、空き領域にどのように分割されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再び `/context` を入力して結果を比較します。会話が短い場合は、大きく変化しないことがあります。

> [!NOTE]
> コンテキスト ウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。圧縮のタイミングを指定するには `/compact` を使います。無関係なタスクに切り替えるときに新しい会話を開始するには、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意な作業が異なり、開発者によって好みも違います。Copilot CLI では、利用できるモデルを確認して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学びを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、用途に合った出力先を選択します。たとえば `/share file` ではローカルに Markdown ファイルをエクスポートできます。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は省略可能です。チームが共有する意図のないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業を拡張または委任する

ワークショップの基本的な作業は完了です。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに割り当て、並列に実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` はタスクを Copilot cloud agent に送り、非同期で作業して pull request を開けるようにします。

追加の作業やリモートでの作業が発生する可能性があるため、これらのコマンドは省略可能です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業を詳しく確認するには、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI の slash command を使うと、構成を変更し、セッションを共有し、内部の状態を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認しました。

Copilot CLI には、ほかにも多くの slash command があります。続けて[学んだことと次のステップを振り返ります][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
