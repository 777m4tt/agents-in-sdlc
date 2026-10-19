---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。作業の進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにします。Copilot に特定のコンテキストを共有する方法はいくつかあります。主な方法の1つは[指示ファイル][instruction-files]です。コード生成に関するガイダンスを伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを限定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を使って、フィルター機能の最初のデータ スライス（発行者ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。
- 同じプロンプトを再度送り、生成コードが新しい標準を採用する様子を確認します。
- 次の演習で更新済みガイダンスを Copilot が利用できるよう、指示の更新とフィルター機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わない場合があります。Copilot は確率的に動作します。この演習の目的は、指示を更新した後の**傾向**を確認することであり、出力を文字単位で一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発の進め方に関するガイドラインと要件があります。

- データ層には常にユニット テストが必要です。
- UI はダーク モードで、現代的なデザインにします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディング スタイルや要件を Copilot に伝えるコンテキストです。好みのコーディング規約、ライブラリ、コメントの書き方などを指定して、より適切な提案やコード スニペットを得られます。リポジトリ全体または特定のファイル種別に対する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、構築中のものの概要、ベスト プラクティスなど、多くのチャットや CLI リクエストに関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別（TypeScript や Astro など）に対するガイドラインを指定します。UI コンポーネントやユニット テストの作成など、タスク別の指示にも使えます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方をコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[カスタム指示の対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

このワークショップですべての作成方法を扱うことはできませんが、サンプル プロジェクトに代表的な方法があります。

- `copilot-instructions.md` には、構築するものの概要、プロジェクト構造、共通のコーディング標準など、プロジェクト全体のガイダンスを記述します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ層などのファイル種別や特定のタスクに対する指示に使用します。
- 自然な言葉で明確なガイダンスを書き、良いコード例と避けるべき例を示します。

指示ファイルの作成方法や AI の使い方に唯一の正解はありません。試行を通じて、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があります。
>
> Copilot を使って指示ファイルを生成することもできます。各環境で操作は異なります（例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。利用中の環境に関係するレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目します。これらの指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに対する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドはリポジトリ ルートからの相対 glob で、指示の適用対象を決めます。ここでは `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールし、`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分け、コード生成時に参考にする例を Copilot に示せます（パスはリポジトリ ルートではなく指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。これから TSDoc コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送ります。後で指示を更新し、同じプロンプトをもう一度送って違いを確認します。

1. 前の演習で開いたエディター タブをすべて閉じ、Copilot が必要なコンテキストだけを読み取るようにします。
2. Copilot がヘルパーの配置場所を把握できるよう、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査し、ヘルパー ファイルやテストなどにコード変更を適用する様子を確認します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、発行者の型付き配列を返す型付き関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 提案されたコードに TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まるかを確認することが目的です。

## リポジトリ全体の標準を追加する

`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に伝えます。コード提案を改善できるよう、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを見つけます。コーディング標準は記載されていますが、コード内ドキュメントのルールはまだありません。
3. 既存の標準の直後に、ファイル コメント ヘッダーと TSDoc コメントの追加を Copilot に指示する次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、全体に適用するリポジトリ レベル（`.github/copilot-instructions.md`）にも、言語、ファイル種別、タスクに応じた `*.instructions.md` にも作成できます。ドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再送信して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話のバッファーをクリアします。
2. Copilot が適切なファイルに注目するよう、`src/lib/publishers.ts` を再度選択します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用させました。次の演習では、この実際のコードをさらに発展させます。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリ内のほかの資産と同様に、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. 次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎に関して Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連ファイル（テストなど）が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたので、バックログのドキュメント標準に関する Issue と関連付けた pull request を作成します。GitHub ツールが接続されているため、Copilot に Issue を見つけて PR を作成し、マージするよう依頼できます。

> [!NOTE]
> Copilot が GitHub 上で操作するのは、これが初めてです。プロジェクト テンプレートでは GitHub ツールがワークスペースにあらかじめ接続されています。セットアップは不要ですが、Copilot が初めてツールを使うときに VS Code から GitHub へのサインインを求められる場合があります。プロンプトに従って許可してください。この接続（Model Context Protocol）については、後の演習で学びます。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と、フィルター機能のために追加した発行者ヘルパーの基礎の両方を説明する PR を現在のブランチから作成し、該当 Issue を解決するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索や PR の作成を始めます。
5. Copilot が GitHub の操作を行うため **Allow** を求めたら、コマンドを確認し、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR のマージとローカル ブランチの `main` への切り替えを依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub の操作やシェル コマンドの実行を許可するよう求めたら、コマンドを確認し、適切な場合に **Allow** を選択します。

GitHub Copilot の支援を受けて、pull request を作成してマージしました。

## まとめと次のステップ

このプロジェクトの指示ファイルを Copilot がどのように利用するか確認し、VS Code の Copilot Chat で次のことを行いました。

- リポジトリのカスタム指示とパスを限定した指示ファイルを確認し、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認しました。
- 現在の指示を使ってプロンプトを送り、フィルター機能の最初のデータ スライスである発行者ヘルパーを生成しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再度送り、生成コードに新しい標準が適用される様子を確認しました。
- 次の演習で Copilot が更新済みのガイダンスを使えるよう、指示の更新とフィルター機能の一部を `main` にコミットしました。

次に、[エージェント モード][next-lesson]で指示を活用し、Copilot にコードベース全体で新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [より良いカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
