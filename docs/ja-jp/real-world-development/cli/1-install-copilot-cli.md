---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、操作に慣れて、用意されたフィルター Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースの探索、コードの生成、コマンドの実行、外部ツールとのやり取りをすべてコマンド ラインから行えます。タスクを任せて変更を依頼しながら、作業に集中できます。最初のステップはツールのインストールです。すでに使い慣れたツールを使ってインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを使ってフィルター Issue を探します。

## シナリオ

チームは、増加するバックログに対応するため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に使うターミナルでエージェントを利用できます。このレッスンでは、Copilot CLI をインストールして認証し、ワークショップ全体で利用できる状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew を使ってインストールできます。GitHub Codespaces には Node.js がインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされていて、必要なバージョンを満たすことを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初めて起動すると、Copilot CLI が GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められたら、device code の指示に従って Copilot CLI を認証し、アクセスを許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。認証を求められずに Copilot CLI が起動した場合は、そのまま進めて問題ありません。

## 操作に慣れる

通常のシェル プロンプトで実行したコマンドは、Codespace で直接実行されます。Copilot CLI を起動すると、自然言語はエージェントに送られ、slash command で会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力し、インストール済みのバージョンで利用できるコマンドを確認してから、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の簡単な質問を Copilot に送り、正常に動作することを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、回答の前に Copilot がリポジトリを調査することを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のプロンプトで、フィルター Issue を探すよう Copilot に依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の変更を行った後、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。worktree は自動的に作成しません。それぞれの変更を始める前に、機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は、設定済みの権限外でツールを使う前に確認を求めます。このワークショップでは `--yolo` を付けて Copilot CLI を再起動します。これはユーザーが承認するショートカットで、Codespace 内で操作のたびに確認を求められないようにし、演習に集中できます。

> [!CAUTION]
> `--yolo` は完全な自動権限（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や VM のような分離された環境でのみ使用し、日常的な開発の既定値として alias を設定しないでください。詳しくは[ツールの許可と拒否][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` を付けると、後のレッスンで Issue や pull request を操作するために必要な GitHub MCP の読み取り・書き込みツールが有効になります。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` を入力して Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトに関する簡単な質問をもう1つ送り、会話が動作することを確認してから、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後に、`copilot --resume` を使って同じ会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使ってフィルター Issue を見つけました。

次に、Copilot CLI を使ってゲーム カードに星評価を表示する[最初の変更を行います][next-lesson]。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
