---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。作業の進め方に決まりがある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]は、どのようなコードを作るかだけでなく、コードの構成方法も伝えます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスを限定した指示ファイルがエージェントに伝わる方法を確認します。
- コーディング標準に従うよう指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発の進め方に関するガイドラインと要件があります。

- コメントは、コードの言い換えではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。注入可能な `db` 引数がある場合は、その説明も含めます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変わったらコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示は、コーディング スタイルや要件を Copilot に伝えるコンテキストです。好みのコーディング規約、ライブラリ、コメントの書き方などを指定し、より適切な提案やコード スニペットを得られます。リポジトリ全体、または特定のファイル種別に対する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに対するガイドラインを指定します。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、影響を比較できるよう指示ファイルが用意されています。変更を加える前に内容を確認します。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、このリポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに対する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドの glob によって、指示を適用するファイルが決まります。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を小さな再利用可能なファイルに分け、コード例を Copilot に示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

用意された指示ファイルはよい出発点ですが、まだ不足があります。中心となる `copilot-instructions.md` を変更して、新たに生成する TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で、35行目付近にある **Code formatting guidance** セクションを見つけます。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。機能のコンテキストを保ったまま新しいガイダンスを読み込めるよう、指示を保存した後にフィルターの会話を終了して再開します。

1. フィルターの会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替えた状態から、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたら、フィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルをどのように読み込むかを確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスを限定した指示ファイルがエージェントに伝わる方法を確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次に、[再利用可能な quality-checks skill をカスタマイズして実行し][next-lesson]、テストと lint を一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
