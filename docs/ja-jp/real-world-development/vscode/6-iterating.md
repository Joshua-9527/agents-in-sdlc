---
title: "演習 6 - GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
| :-- |

## 作業をレビューする

このラボでは、ユーザー体験を改善するいくつかのタスクに GitHub Copilot と取り組んできました。エージェントモードでクライアントとサーバーの両方にフィルタリングを追加し、Playwright Model Context Protocol (MCP) サーバーでブラウザーから手動テストを行い、カスタムエージェントを使ってハイコントラストモードとライトモードを追加しました。さらに、セッション中にエージェントに指示して作業を拡張しました。次はローカルの作業を公開し、チームと同じ方法でレビューします。

### シナリオ

生成型の人工知能 (AI) を導入しても、ソフトウェア設計と開発・運用 (DevOps) の基本は変わりません。Copilot が生成した内容も、通常のレビューサイクルに沿って確認します。Codespace からアクセシビリティの変更を push し、pull request (PR) を開いて、チーム全体に参加してもらう前に差分を確認しましょう。

## アクセシビリティ機能を公開する

[演習 4][exercise-4]と[演習 5][exercise-5]でアクセシビリティカスタムエージェントを使って実装した、ハイコントラストとライトモードの切り替えは、commit 済みの変更として Codespace にあります。ブランチを push して pull request を開き、チームにレビューしてもらいます。

1. Codespace に戻ります。
2. VS Code で **Source Control** ビューを開きます。
3. アクセシビリティの変更が commit 済みであることを確認します。演習 5 で未 commit の変更がある場合は、ここでステージして、`Add high-contrast and light-mode toggles` など、内容が分かるメッセージで commit します。
4. **Publish Branch** を選択してブランチを公開します。または **...** メニューから **Push** を選択します。
5. VS Code から github.com で新しいブランチを開くよう促されたら、そのプロンプトを受け入れます。プロンプトが表示されない場合は、リポジトリに移動してブランチのバナーから **Compare & pull request** を選択します。
6. 分かりやすいタイトル（例: `Add high-contrast and light-mode toggles`）を設定し、何を、なぜ変更したかを短く説明します。
7. **Create pull request** を選択します。
8. PR を開いたら、**Files changed** タブを選択して変更全体をレビューします。特に次の点を確認します。
   - モード切り替え用のユーザーインターフェイス (UI) コンポーネント。
   - 利用者の設定を保存する local storage の使用。
   - ハイコントラストモードとライトモードの Cascading Style Sheets (CSS) またはスタイルの変更。
   - アクセシビリティ属性（Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など）。
   - モード切り替えを管理する JavaScript / TypeScript コード。

9. **Conversation** タブに戻ります。
10. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. ワークフローが完了するまで待ちます。問題がなければ、成功したことを確認できます。

> [!TIP]
> アクセシビリティの作業について別の意見が必要な場合は、PR コメントで `@copilot` にメンションして「この PR に追加の Web Content Accessibility Guidelines (WCAG) の問題がないかレビューしてください」や「キーボード操作の改善を提案してください」などと依頼します。Copilot はコメントに対応する新しいセッションを開始します。

## 任意の演習 - ローカルでさらに試す

統合開発環境 (IDE) でエージェントと反復して作業するには、実際に繰り返す必要があります。VS Code で追加セッションを行う場合は、次のタスクを試します。

1. ゲーム詳細ページに支援者向けの問い合わせフォームを追加します。
2. ゲーム一覧ページにページネーションを実装します。
3. `src/lib/` のデータアクセスヘルパーに入力検証とエラー処理を追加します。
4. アクセシビリティエージェントの対象範囲を広げます。たとえば、サイト全体のキーボードフォーカスの順序を監査します。

## まとめ

おめでとうございます。VS Code ハーネスを完了しました。このラボでは、次のことを行いました。

- **Playwright MCP で機能を手動テストしました。** Playwright MCP サーバーを追加し、pull request を開く前に Copilot にブラウザーを操作させてフィルタリング機能を検証しました。
- **エージェントモードでスタック全体にわたる変更を連携して行いました。** 1 つのセッションで、クライアント、サーバー、テストにわたるフィルタリング機能を追加しました。
- **カスタムエージェントを使いました。** エージェント選択からアクセシビリティに特化したカスタムエージェントを選び、リポジトリの指示に沿ってハイコントラストモードを実装する様子を確認しました。
- **エージェントセッションを管理して方向付けました。** 提案された変更をインラインでレビューして必要な変更を採用し、ライトモードの追加依頼でセッションを拡張しました。
- **pull request で一連の作業を締めくくりました。** ローカルの作業を公開し、チームと同じ方法で全体をレビューしました。

## レビューと次のステップ

必須の VS Code ハーネスはこれで完了です。ここでワークショップを終了できます。

Copilot のエージェント機能についてさらに学びたい場合は、別のハーネスで異なる操作環境から関連するシナリオを確認できます。

- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、スキル、QA カスタムエージェント、Playwright MCP、Agent Merge、コンテキストを考慮した引き継ぎを使い、レビュー済みの 2 つの変更をターミナルからリリースします。
- ☁️ **[Cloud agent ハーネス](../../cloud/)** — issue の cloud agent への割り当て、エージェントページでのセッション監視、pull request での非同期な反復作業に取り組みます。

ここで始めた作業を続けることもできます。[awesome-copilot][awesome-copilot] には、自分のプロジェクトに応用できる指示ファイル、カスタムエージェント、スキルが豊富にあります。

任意の拡張演習である[任意: Foundry を組み込む][exercise-7]では、VS Code と Microsoft Foundry Toolkit を使ってモデルを準備し、Backer Concierge をデプロイしてサイトに接続します。

## リソース

- [GitHub Copilot][github-copilot]
- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェントモードを使う][agent-mode]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
