---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシ コード、実装する時間がなくバックログに残った機能要望など、さまざまな課題があります。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、エージェントは人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を委任して開発を加速し、開発者はより大きな課題に集中できます。

GitHub Copilot cloud agent を使って、次の操作を行います。

- コード生成用の環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- issue の範囲を明確にする重要性を学ぶ。
- issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを開発した委託先は、ドキュメントを十分に作成していませんでした。まず、アプリケーション内のエクスポートされたすべての関数に TSDoc の doc comment を追加します。

さらに、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示して、利用者がほかのゲームも見続けられるようにします。現時点では完成された実装は不要で、UX の受け入れテストに使える状態にしたいと考えています。この作業はブロッカーですが、ほかに優先度の高い issue があります。

どちらも優先順位が下がりやすいタスクの例であり、GitHub Copilot cloud agent に適しています。Copilot cloud agent に非同期で作業させれば、開発者は別の作業に集中し、あとから戻って作業をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると Copilot は進捗を記録するドラフト プルリクエストを作成し、環境を準備して、タスクに着手します。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省けるわけではありません。一見簡単な作業でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階を踏んで学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、カスタム命令と、Copilot の作業に命令がどう影響するかを確認しました。カスタム命令の演習を完了していれば `.github/copilot-instructions.md` を実際に使ったことがあるでしょう。まだの場合は、この機会に簡単に確認してください。GitHub Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる instruction ファイルを読み取り専用で確認して、あとでその効果を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc の doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この命令は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てると、作成されたプルリクエストに TSDoc の doc comment、TypeScript の規約、コメント ヘッダーが反映されているか確認します。後の演習で PR をレビューするときにも、これらの点を確認します。

## GitHub Copilot cloud agent の開発環境をセットアップする

コードを書くときは、開発環境を準備して、必要なセットアップ スクリプトを実行することがよくあります。Copilot にタスクを割り当てる場合も同じです。Copilot はソフトウェア エンジニアのように作業します。

Cloud agent は [GitHub Actions][github-actions] を使って作業環境を用意します。作業を開始する前に実行する[セットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。このラボの手順を簡潔に進められるよう、あらかじめ設定済みです。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が使えるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順* に必要な最小限の権限だけを設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # 依存関係をインストールするなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動でクローンします。
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

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、次の点を確認してください。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動くかテストするのに便利です。

## ドキュメントを追加する

ドキュメントが重要だと誰もが理解していますが、情報が古い、またはまったくないプロジェクトも少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、cloud agent に任せるのに適した作業です。バックグラウンドで必要なドキュメントを生成します。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot の ToDo リストが表示されます。

> [!NOTE]
> Copilot の ToDo リストが PR に表示されるまで数分かかることがあります。Copilot が環境を準備し (前に説明したワークフローを実行し)、プロジェクトを分析して、作業の進め方を決めています。

13. リストと、実行されるタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ウィンドウの再読み込みが必要な場合があります。

16. Copilot の進行中のセッションをスクロールして、問題をどう解決しているか確認します。コードを調べて状態を理解し、方針を考えて、コードを作成する様子を確認できます。

この処理には数分かかります。GitHub Copilot cloud agent の主な目的の 1 つは、非同期で作業して、別の作業に集中できるようにすることです。この機能を活用するため、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加するコードを書きます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは作業を分けられることです。デザイン チーム向けに関連ゲームを追加する作業は長時間かからないかもしれませんが、その時間をほかの作業に使えます。このタスクも GitHub Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
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
8. 右側にある **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![issue の作業中であることを目の絵文字で示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。GitHub Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はなく、完了後にレビューできます。次はカスタム エージェントの作成と使用を確認しましょう。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に issue を割り当て、非同期で作業させることができます。技術的負債の解消、新しい機能の作成、別のフレームワークへのコード移行などに活用できます。

次の概念を学びました。

- コード生成用の環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- issue の範囲を明確にする重要性を学ぶ。
- issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントを作成して使う方法を確認します。[Copilot cloud agent は MCP サーバーも使えます][cloud-agent-mcp]。また、前のモジュールで確認したカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
