---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成型の人工知能 (AI) を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot が何をするかだけでなく、どのように作業するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加します。

> [!NOTE]
> VS Code やコマンドラインインターフェイス (CLI) のハーネスとは異なり、この演習では指示を変更する前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub issue を非同期に処理するため、影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で、cloud agent が作成した pull request をレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには、必ず単体テストを用意します。
- ユーザーインターフェイス (UI) はダークモードにし、現代的な見た目にします。
- TypeScript Documentation (TSDoc) のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストを Copilot に提供できます。規約、ライブラリ、コメントの書き方などを指定して、適切な提案やコード生成につなげられます。リポジトリ全体に適用する指示も、特定のファイルだけに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべての要求で Copilot に送られます。技術スタック、開発中のものの概要、ベストプラクティスなど、多くの要求に関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類向けに作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに関するガイドラインを記載します。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、別の規格に基づく指示ファイルにも対応しています。詳細は[カスタム指示のサポート状況][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

サンプルプロジェクトの指示ファイルは、一般的な管理方法を示しています。主なポイントは次のとおりです。

- `copilot-instructions.md` には、開発中のもの、プロジェクト構成、全体のコーディング規約など、プロジェクトレベルの情報を記載します。
- `*.instructions.md` ファイルは、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種類や特定のタスク向けに使います。
- 自然な言葉で、分かりやすく指示します。コードの良い例と避けるべき例も示します。

指示ファイルの作成方法や AI の使い方に唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。具体的な手順は、各ハーネスのレッスンで説明します。
>
> テンプレートや出発点を探すには、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリにある、1 つの `copilot-instructions.md` と、タスク別の複数の `*.instructions.md` ファイルを確認します。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内には、**GitHub Actions Workflows** の指示もあります。これらの指示は、Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対 glob で適用対象を指定していることを確認します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。これにより大きな指示を小さく再利用可能なファイルに分け、コード生成の例を Copilot に示せます。ここにあるパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する指示はまだありません。次の手順で TSDoc コメントとファイルヘッダーの規約を追加します。

## リポジトリに新しい規約を追加する

この演習では、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーをコードに追加する、プロジェクト全体の規約を登録します。後の演習で issue を割り当てると、cloud agent はこの指示を読み込みます。

編集の前に、変更用のブランチを作成します。cloud agent は issue の対象ブランチから指示を読みますが、ここでは変更を個別に commit します。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、プロジェクトレベルの情報を Copilot に提供するためのファイルです。コーディング規約を記載して、コードの提案を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の規約を確認し、コード内ドキュメントのルールがないことを確認します。
3. 既存の規約の直後に、ファイルヘッダーと TSDoc コメントを追加する次の Markdown を記述します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリ全体の指示には `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスクには `*.instructions.md` を使います。コードコメントの規約など、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の変更を commit、push、マージする

cloud agent は issue の対象ブランチから指示ファイルを読み込みます。次の演習で Copilot に issue を割り当てると、`main` からブランチが作成されます。cloud agent が新しい指示を使えるように、この変更を `main` にマージします。

1. 変更をステージして commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けた pull request を開き、マージします。次の演習で issue を割り当てる際、cloud agent は更新された指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せず、変更を `main` に直接 commit します。ここでは実際のプロジェクトでの手順に合わせるためにブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の規約を追加しました。このハーネスの後半で cloud agent が作成する pull request に、その規約が反映されます。

次は[最初の issue を Copilot cloud agent に割り当て][next-lesson]ます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
