---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要なものを準備します。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を起動します。次のレッスンでは、codespace の統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次のことを行います。

- Tailspin Toys プロジェクトの自分用コピーをテンプレートから作成します。
- Codespace を作成し、プロジェクトの準備ができていることを確認します。

## 演習用リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使って作業します。[テンプレート リポジトリ][tailspin-template]から作成します。新しいリポジトリには、演習で必要なすべてのファイルが含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template** を選択し、続けて **Create a new repository** を選択して、自分用のリポジトリ コピーを作成します。
3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。
4. ワークショップの後半で参照するため、作成したリポジトリのパス（`organization-or-user-name/repository-name`）を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップ全体でこれらの Issue を使って作業します。自分で Issue を登録する必要はありません。

ワークショップ テンプレートの新しいコピーを使ってください。このテンプレートにはリポジトリの指示、アプリケーション コード、テスト、`quality-checks` skill、演習で使うバックログが含まれています。古いコピーを使う場合は、必要なファイルが含まれているかファシリテーターに確認してください。

## Codespace を作成する

次に、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードの作成、実行、デバッグができるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応したフル機能のエディターを利用できます。

1. 作成したリポジトリを開きます。
2. **Code** を選択します。
3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカル データベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と表示された場合は、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

セットアップが完了しました。このレッスンでは、次のことを行いました。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成しました。
- Codespace を作成し、プロジェクトの準備ができていることを確認しました。

次に、Codespace に [GitHub Copilot CLI をインストールします][next-lesson]。その後、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
