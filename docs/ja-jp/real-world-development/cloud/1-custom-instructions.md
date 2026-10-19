---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。作業の進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにしておきます。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように進めてほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを限定した指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを使いません。Copilot cloud agent は GitHub Issue を非同期に処理するため、リアルタイムで並べて影響を示すのは困難です。このハーネスの後半で、cloud agent が作成した pull request をレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発の進め方に関するガイドラインと要件があります。

- データ層には常にユニット テストが必要です。
- UI はダーク モードで、現代的なデザインにします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示は、コーディング スタイルや要件を Copilot に伝えるコンテキストです。好みのコーディング規約、ライブラリ、コメントの書き方などを指定して、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のファイル種別に対する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、構築中のものの概要、ベスト プラクティスなど、多くのチャットや CLI リクエストに関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別（TypeScript や Astro など）に対するガイドラインを指定します。UI コンポーネントの作成やユニット テストの作成など、タスク別の指示にも使えます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方をコード生成に使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[カスタム指示の対応状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

このワークショップでは指示ファイルの作成方法をすべて扱いませんが、サンプル プロジェクトでは代表的なアプローチを紹介します。

- `copilot-instructions.md` には、プロジェクトの説明、リポジトリ構造、共通のコーディング標準など、プロジェクト全体のガイダンスを記述します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ層など、ファイル種別や特定のタスクに対する指示に使用します。
- 自然な言葉で、明確なガイダンスを書きます。コードの良い例と悪い例を示します。

指示ファイルの作成方法や AI の使い方に唯一の正解はありません。試行を通じて、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用のファイルがあります。
>
> Copilot を使って指示ファイルを生成することもできます。各環境で操作は異なります（例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。必要な場合は、利用中の環境に対応するレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、さまざまなタスク用の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。これらは、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドの glob（リポジトリ ルートからの相対パス）によって指示の適用対象が決まります。ここでは、`**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾までスクロールし、`unit-tests.instructions.md` などの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を小さな再利用可能なファイルに分け、コード生成時に参考にする例を Copilot に示せます（パスはリポジトリ ルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## リポジトリの標準を追加する

ここでは、TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーをコードに含めるという、プロジェクト全体のルールを追加します。後続の演習で Issue を割り当てるとき、cloud agent がこのルールを参照します。

編集の前に、作業用ブランチを作成します（cloud agent は Issue の対象ブランチにある指示を読み取りますが、ここでは編集を整理してコミットします）。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` はプロジェクト全体の情報を Copilot に伝えます。コード提案を改善できるよう、コーディング標準を記述します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを見つけます。コーディング標準が記述されていますが、生成コードにドキュメント コメントがなかった理由となる、コード内ドキュメントのルールはありません。
3. 既存の標準の直後に、次の Markdown 行を追加して、ファイル コメント ヘッダーと TSDoc ドキュメント コメントの追加を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル（`.github/copilot-instructions.md`）に置いて全体に適用することも、言語、ファイル種別、タスクに応じた `*.instructions.md` ファイルとして作成することもできます。ドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチの指示ファイルを読み取ります。次の演習で Copilot に Issue を割り当てると、Copilot は `main` からブランチを作成します。指示を使えるようにするには、変更を `main` にマージします。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けた pull request を開いてマージします。次の演習で Issue を割り当てるとき、cloud agent がこの指示を読み取れるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業することもできます。その場合、ブランチ作成の手順を省略し、`main` に直接コミットできます。ブランチを作成する手順は、実際のプロジェクトでの進め方を体験するために含まれています。

## まとめと次のステップ

このプロジェクトの指示ファイルを Copilot がどのように利用するかを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で、cloud agent が作成する pull request にその標準が反映されることを確認します。

次に、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
