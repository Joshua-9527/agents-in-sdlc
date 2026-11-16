---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能要望などがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行します。完了すると、人間の開発者がレビューできるドラフトの pull request (PR) を作成します。面倒な作業を任せて開発を加速し、より大きな課題に集中できます。

このレッスンでは、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- issue の範囲を明確にする重要性を理解します。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys では、解決したい技術的負債があります。最初のサイトを構築した委託先がドキュメントを残しておらず、ほとんど何もありません。まず、アプリケーションのエクスポートされたすべての関数に TypeScript Documentation (TSDoc) のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーの他のゲームを表示し、利用者がさらにゲームを探せるようにします。洗練された実装はまだ必要ありません。ユーザー体験 (UX) の受け入れテストに使える状態が目標です。この作業は現在ブロックされていますが、ほかに優先度の高い issue があります。

どちらも、優先度が下がりやすいタスクです。Copilot cloud agent に割り当てれば、開発者が別の作業に取り組んでいる間に非同期で処理され、後から結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当て後、Copilot は進行状況を追跡するドラフト pull request を作成し、環境を準備してタスクに取り組みます。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、pull request で通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作にも複雑さが潜んでいるため、[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。人工知能 (AI) を使ったペアプログラミングとして、段階的に作業し、学び、試し、適応させます。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイドに使う方法を学びました。まだ実際に確認していない場合は、ここで `.github/copilot-instructions.md` を簡単に確認します。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認しておくと、後で指示の影響を見つけやすくなります。

リポジトリのファイルを読み取り専用で確認し、cloud agent が利用する指示を把握します。

1. GitHub Web UI または起動中の Codespace で `.github/copilot-instructions.md` を開き、**Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントの要件を確認します。
2. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、TSDoc コメント、TypeScript の規約、コメントヘッダーが反映されているかを確認します。後の演習で PR をレビューするときにも、改めて取り上げます。

## Copilot cloud agent の開発環境をセットアップする

コードの作成には、通常、必要な開発ツールや依存関係を準備する環境設定が必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てる場合も同様です。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業前に必要な開発ツールや依存関係を用意できます。このラボの手順を簡潔にするため、あらかじめ設定済みです。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを準備します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順* に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順でリポジトリをクローンする場合は `contents: read` アクセス許可が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用のデータを用意するため、ローカルの SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと同じ形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot が PR に取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されているため、リポジトリの **Actions** タブから手動で実行できます。Copilot が実行するまで待たずに、ワークフローが成功するか確認できます。

## ドキュメントを追加する

ドキュメントが重要なのは広く認識されていますが、情報が古かったり不足していたりするプロジェクトもあります。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成にも役立つため、この issue は Copilot cloud agent に依頼するのに適しています。後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

  ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は先ほどのワークフローを実行して環境を準備し、プロジェクトを分析して、最適な取り組み方を検討しています。

13. todo リストを読み、Copilot が実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が確認できるように、ウィンドウの再読み込みが必要な場合があります。

16. ライブセッションをスクロールして作業の様子を確認します。Copilot がコードを調査して状態を把握し、計画を考えて判断した後、コードを作成する様子を確認できます。

作業には数分かかる場合があります。Copilot cloud agent の主な利点は、タスクを非同期に実行し、開発者が別の作業に集中できることです。もう 1 つのタスクを Copilot cloud agent に割り当ててから、アプリへの機能追加に取り組みましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。開発者があるタスクに取り組んでいる間、エージェントは別のタスクを進められます。デザインチーム向けに関連ゲームを追加する作業が短時間であっても、その間に別の作業を進められます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して issue 作成ダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、最初のコメントに 👀 が表示され、Copilot が作業を始めたことが分かります。

![issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューします。次はカスタムエージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、issue を Copilot に割り当て、非同期で処理させることができます。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などに活用できます。

このレッスンでは、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- issue の範囲を明確にする重要性を理解しました。
- issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも使えます][cloud-agent-mcp]。また、前のモジュールで扱ったカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
