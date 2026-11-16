---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に合わせた人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、対象タスクに効果的に取り組めるよう、重点を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を Copilot に提供します。チームの知見を再利用可能なエージェントにできます。たとえば、Web Content Accessibility Guidelines (WCAG) への準拠を確認するアクセシビリティエージェント、安全なコーディング手法に従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割が一部重複しています。どちらも主に Markdown ファイルで定義し、操作方法を AI に指示します。最も分かりやすい違いは、**カスタムエージェントが作業者で、スキルはツール**だということです。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルや他のエージェントを調整できます。このラボのアクセシビリティカスタムエージェントは、サイトをアクセシビリティガイドラインに照らして確認し、更新します。その作業の一部として、pull request ワークフローやテスト実行を管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI を使う他の作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディングプラットフォームを使えるようにしたいと考えています。最近のフィードバックによると、テキストと背景色のコントラストが不十分で、現在のダークテーマを読みづらい人がいます。デザインチームは、利用者が切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティが重要なため、カスタムエージェントを使って機能をすばやく作成します。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントがすでに用意されています。内容を確認して、Copilot への指示を理解します。

演習 3 のフィルタリング pull request (PR) と分けて作業できるよう、Codespace で `main` からアクセシビリティ作業用のブランチを作成してから、カスタムエージェントの指示を確認します。

1. Codespace に戻り、ターミナルを開いて次のコマンドを実行します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドが含まれる YAML frontmatter を確認します。

> [!CAUTION]
> `name` と `description` を含む frontmatter は、カスタムエージェントに必須です。

4. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責任。
   - アクセシビリティのベストプラクティス。
   - Hypertext Markup Language (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント一覧には、`.github/agents` で定義されたすべてのカスタムエージェントが表示されます。エージェントを選択すると、その指示とツールに基づくチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Copilot Chat を開き、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な選択は各手順で説明します。
>
> 1. Codespace に戻ります。
> 2. Copilot Chat パネルを開き、**New Chat** を選択します。

1. Chat ビューのエージェント一覧で、まだ選ばれていない場合は **Agent** を選択します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント一覧を選択します。現在のエージェント名が表示されます。既定の名前は **default** です。
3. 使用可能なエージェント一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトのファイルを編集します。編集ごとに、ファイルパスと差分がチャットに表示されます。
6. 編集が反映されるたびに、エージェントが変更した作業ファイルを示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを実際に変更します。

次の演習では、作業中の変更を確認し、方向を調整します。

## まとめと次のステップ

[GitHub Copilot のカスタムエージェント][custom-agents]は、特定のタスクや分野に合わせた専門的な AI アシスタントです。カスタムエージェントを使うと、チームの知見や規約を再利用可能なエージェントにまとめ、Copilot が特定の種類の作業を効果的に行えるようにできます。

このレッスンでは、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat のエージェントモードでカスタムエージェントを使いました。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認しながら、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
