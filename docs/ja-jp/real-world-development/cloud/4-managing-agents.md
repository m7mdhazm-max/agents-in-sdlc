---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの 2 つの演習では、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを GitHub Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。たとえば GitHub.com の[agents ページ][agents-page]では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次の操作を行います。

- agents ページで cloud agent のタスクを監視する。
- 実行中のセッションに追加機能を依頼する。

## シナリオ

ハイ コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイル更新と切り替え機能を開発しているため、同時に追加するのが合理的です。ハイ コントラスト モードだけでなくライト モードも追加するよう、エージェントの作業に指示を出します。

## GitHub Copilot cloud agent のタスクを確認する

GitHub Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左側のペインにあるタスク一覧を確認します。次のようなタスクが表示されます。
    - コードベースのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれます。
    - リポジトリからコードをチェックアウトする。
    - Copilot の作業環境を作成する。
    - MCP サーバーをセットアップする。
    - 割り当てられたタスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業をさらに監視できます。

## Cloud agent に追加の指示を出す

実行中のタスクを確認できたので、cloud agent がハイ コントラスト モードに取り組んでいる間に、ライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードのセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control に表示される Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで数分間、セッションを確認します。次のようなメッセージが表示されたら作業開始の目安です。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページの cloud agent タスクで、Steer active session while Copilot is working ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に追加したことを確認します。

## Copilot の作業を待つ

これまでと同様に、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えると、新しい依頼を作業の流れに取り込みます。

作業には数分かかります。その間に、これまでに学んだ内容を振り返ってみましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視して指示する中心的な場所である Copilot agents ページを確認しました。mission control では、実行中と完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクに指示を追加できます。

次の概念を学びました。

- mission control と agents ページで cloud agent のタスクを監視する。
- 実行中のセッションに追加機能を依頼する。

Copilot がアクセシビリティ機能を実装している間に、次は [Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
