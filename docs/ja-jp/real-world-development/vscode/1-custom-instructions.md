---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成型の人工知能 (AI) を使うときは、コンテキストが重要です。特定の方法でタスクを進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、その情報を参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。中でも重要なのが、コード生成の指針となる[指示ファイル][instruction-files]です。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント規約が Copilot に伝わる仕組みを確認します。
- 現在の指示を使って、フィルタリング用データスライスの最初の部分（パブリッシャーヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加します。
- 同じプロンプトを再実行し、生成コードに新しい規約が反映されることを確認します。
- 指示の変更とフィルタリングの作業を `main` に commit し、次の演習で新しい指針を利用できるようにします。

> [!CAUTION]
> 生成コードが設定済みの規約から外れる場合があります。Copilot は確率的に動作します。この演習では、出力を一字一句一致させるのではなく、指示を更新した後の傾向の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには、必ず単体テストを用意します。
- ユーザーインターフェイス (UI) はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストを Copilot に提供できます。規約、ライブラリ、コメントの書き方などを指定して、適切な提案やコード生成につなげられます。リポジトリ全体に適用する指示も、特定のファイルだけに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべての要求で Copilot に送られます。プロジェクトの技術スタック、開発中のもの、ベストプラクティスなど、大半のチャットや CLI 要求に関係する情報を記載します。
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

## 変更用ブランチを作成する

変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な選択は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトをもう一度送信して違いを確認します。

1. 前の演習で開いたエディタータブをすべて閉じ、Copilot に必要なコンテキストだけを渡します。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェント一覧から **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。ヘルパーファイルとテストが変更されることもあります。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取る型付き関数であり、パブリッシャーの型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加することがあります。その場合も問題ありません。この演習では、指示を更新した後の一貫性の向上を確認します。

## リポジトリに新しい規約を追加する

`.github/copilot-instructions.md` は、プロジェクト全体の情報を Copilot に提供します。コーディング規約を記載して、コードの提案を改善します。

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

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot の対象ファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト規約に沿うよう Copilot を導き、次の演習で利用する実際のコードに適用しました。

## 指示とブランチを commit して push する

指示ファイルもリポジトリ内の他のファイルと同様に、バージョン管理の対象です。変更を commit してブランチをリポジトリに push します。

1. <kbd>Control</kbd>+<kbd>`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎部分として Copilot が変更したすべてのファイルをステージして commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチを push したら、バックログにあるドキュメント規約の issue に関連付けた pull request を作成します。GitHub ツールはすでにプロジェクトに接続されているので、Copilot に issue を検索し、issue をクローズする PR を作成してマージするよう依頼します。

> [!NOTE]
> この演習では、Copilot が GitHub 上で操作するのは初めてです。プロジェクトテンプレートでは GitHub ツールが設定済みなので、セットアップは不要です。Copilot が初めてツールを使うとき、VS Code から GitHub へのサインインを求められることがあります。画面の指示に従って許可してください。後の演習で、この接続に使われる Model Context Protocol を学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新と、フィルタリング作業に備えて追加したパブリッシャーヘルパーの両方を記載した PR を、現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を検索して PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して適切に許可します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェルコマンドの **Allow** を求めたら、内容を確認して適切に許可します。

GitHub Copilot の支援で pull request を作成し、マージできました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を使ったコード生成プロンプトを送り、Copilot の提案を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい規約を追加しました。
- 同じプロンプトを再送信し、生成コードに新しい規約が反映されることを確認しました。
- 指示の変更とヘルパーの基礎部分を commit し、`main` にマージして、次の演習で利用できるようにしました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
