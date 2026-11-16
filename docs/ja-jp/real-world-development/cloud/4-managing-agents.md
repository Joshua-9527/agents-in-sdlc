---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、進行状況を監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この Mission control では、未完了の pull request (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べ、進行中のセッションに指示を追加して、作業の方向を調整できます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 作業中のセッションに指示を出し、機能追加を依頼します。

## シナリオ

チームは、ハイコントラストモードに加えてライトモードも必要だと気付きました。サイトのスタイルと切り替え機能を更新しているため、両方を追加するのが適切です。エージェントの作業にライトモードを追加するよう指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン領域と左側の一覧にあるタスクを確認します。次のようなタスクが表示されます。
   - コードベースのドキュメント更新。
   - ゲーム変更用のアプリケーション プログラミング インターフェイス (API) の生成。
   - Web サイトへのハイコントラストモード追加。
3. 実行中のタスクを 1 つ選択し、Copilot が実行した作業を確認します。次のような内容があります。
   - リポジトリからのコード取得。
   - Copilot の作業環境の作成。
   - MCP サーバーのセットアップ。
   - 割り当てられたタスクを完了するための各種手順。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される pull request (PR) パネルも確認します。PR と変更されたファイルを監視できます。

## cloud agent に指示を出す

実行中のタスクを確認したので、ハイコントラストモードの作業中にライトモードの切り替えも追加するよう Copilot に依頼します。

1. ハイコントラストモードを追加しているセッションを選択します。表示されるタイトルは、Copilot が設定した名前や現在の作業状況によって異なります。

    ![Mission control に表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のセッションを誘導するダイアログが強調表示されたエージェントページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に組み込むことを確認します。

## Copilot に作業を続けてもらう

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順が完了した後、新しい依頼が作業に反映されます。

今回も完了まで数分かかります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視して案内する中心的な場所である Copilot agents ページを使いました。Mission control では、実行中と完了済みのタスクを追跡し、作業を確認し、進行中のタスクに指示して範囲を調整できます。

このレッスンでは、次のことを行いました。

- Mission control とエージェントページで cloud agent のタスクを監視しました。
- 作業中のセッションに指示を追加して機能の拡張を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は[Copilot が作成した pull request を改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
