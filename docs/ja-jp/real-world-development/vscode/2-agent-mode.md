---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は関連ファイルを探して変更し、テストを実行し、問題を修正し、見落としたファイルを見つけて再度変更します。作業はさらに続きます。

Copilot エージェントモードは、こうした作業に役立ちます。

エージェントモードは統合開発環境 (IDE) 内でより自律的に作業します。開発者と同様に、プロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。ここでは、エージェントモードを使ってサイトに機能を追加します。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途を持ちます。エージェントモードは IDE 内で作業し、素早くフィードバックを受けて対話できます。Cloud agent はチームメンバーのように非同期で作業し、issue と pull request を通じてやりとりします。

この演習では、次のことを学びます。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot エージェントモードでユーザーインターフェイス (UI) とデータレイヤーにわたる新機能を実装する方法。
- Copilot エージェントモードが生成した変更とテストをレビューしてからコードベースにマージする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリーやパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャーヘルパーを追加しました。ここでは Copilot エージェントモードで、残りのデータレイヤー、UI、テストを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧と詳細情報を表示します。Astro アプリはビルド時に静的 Hypertext Markup Language (HTML) を生成します。ページは frontmatter 内で Drizzle オブジェクトリレーショナルマッピング (ORM) を使ってローカル SQLite データベースを直接参照します。別のバックエンドのアプリケーション プログラミング インターフェイス (API) やクライアント側のユーザーインターフェイス (UI) フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準ドキュメントオブジェクトモデル (DOM) API を使う、範囲を限定した Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。次の手順で GitHub Codespace から実行します。

1. Codespace に戻ります。前の演習の後で `main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）または <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）を押して、新しいターミナルウィンドウを開きます。
3. フィルタリング用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd>（Mac）または <kbd>Ctrl</kbd>（Windows/Linux）を押しながら選択して Web サイトを開きます。

> [!NOTE]
> Codespace で localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Web サーバーをホストしている Codespace へのプライベートトンネルです。

### Web サイトを確認する

サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧と、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリーなどの詳しい情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な選択は各手順で説明します。

Web サイトの初期実装は動作しますが、さらに機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトを作成すると、issue のバックログも自動的に作成されています。GitHub Copilot に一覧を表示してもらいましょう。

1. Chat ビューのエージェント一覧から **Agent** を選択します。**Agent** は自律的に計画を立て、複数ファイルを変更し、ターミナルコマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者は特定のモデルを推奨するものではありません。ワークショップでは Claude Sonnet 4.5 を使い、比較的一貫した体験になるよう手順に記載しています。ただし人工知能 (AI) と大規模言語モデル (LLM) は確率的に動作するため、提案は記載例と異なる場合があります。これは通常の動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の Model Context Protocol (MCP) コマンドを使うことがありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって有益だと判断した順に、issue を優先順位付けしたことを確認します。

## 指示ファイルを確認する

コードの生成をエージェントに依頼する前に、コンテキストを与える指示ファイルを確認します。Web サイトに機能を追加する方法を説明する[ユーザーインターフェイス (UI)][ui-instructions]の指示ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS (Cascading Style Sheets)、Drizzle などのファイル種類に応じた指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな指示を小さく分け、Copilot に例やテンプレートを示すことができます。

## フィルタリング機能を実装する

フィルタリングを完成させるため、次の項目を更新します。

1. データレイヤー (`src/lib/`) に残りのフィルタリング処理を追加または調整します。
2. フィルタリングの動作を確認するテストを追加または更新します。
3. ゲーム一覧ページにフィルタリング UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェントモードに作業を任せます。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 issue に基づいて機能を実装するよう、次のプロンプトで依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけ、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトファイルを調べる様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイル変更、テスト実行などを行うため、タスクの複雑さやコードベースによって数分かかることがあります。作業中、エディターに **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はありません。完了後、すべての変更に対する **Keep** または **Undo** が表示されます。

7. Copilot に求められたら **Continue** を選択し、テストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に待ち時間が発生したり、テストが失敗したりする場合があります。Copilot は、タスクを完了してエラーがないと判断するまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードで Tailspin Toys に機能を追加する方法を確認しました。この演習では、次のことを学びました。

- Copilot エージェントモードで UI とデータレイヤーにわたる機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う方法。
- Copilot エージェントモードが生成した変更とテストをレビューしてからコードベースにマージする方法。

次は[Playwright MCP サーバーで機能をテスト][next-lesson]し、pull request を作成します。

### 追加の演習 — ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだことを使い、次の点を検討しながら Copilot に実装を依頼します。

1. 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
2. ページサイズを利用者が選択できるようにするか、固定にするかを決めます。
3. 希望する実装に必要なガイダンスをプロンプトに含めます。
4. Copilot にコンテキストを提供して変更を依頼し、必要に応じて改善を繰り返します。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
