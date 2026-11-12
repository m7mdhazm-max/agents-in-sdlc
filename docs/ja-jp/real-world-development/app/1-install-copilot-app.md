---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリです。GitHub Copilot CLI を基盤として GitHub とネイティブに統合されているため、リポジトリ、ブランチ、CI パイプラインをすぐに利用できます。自分ですべての作業を行うのではなく、複数のエージェントを個別の分離されたワークスペースで並行して動かし、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js をインストールしてプロジェクトのコピーを用意できたので、次はアプリをインストールして、そのリポジトリを接続します。

このレッスンでは、次の操作を行います。

- GitHub Copilot app をインストールしてサインインする。
- GitHub リポジトリからプロジェクトをアプリに追加する。
- テンプレートによって用意されたバックログを含め、ワークスペースを確認する。
- クイックチャットを試して、アプリについて質問する。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** ポリシーを有効にしている必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加しましょう。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地を使用する GitHub Enterprise Cloud の場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリを選択するよう求められたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。リポジトリの選択を省略して、オンボーディング後に追加することもできます。
6. テーマを選択するよう求められたら選択し、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを探します。すでに表示されていれば、再度追加する必要はありません。
8. まだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を入力します。
9. リポジトリがサイドバーに表示されることを確認します。このレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとめられ、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの使い方を確認しましょう。サイドバーには主に次の領域があります。

- **New** - Copilot との新しいチャット セッションを開始します。
- **Pull requests** - アプリの GitHub ネイティブ統合を通じてプルリクエストを表示します。アプリを離れずに PR の参照やフィルター、CI 状態の確認、レビューを行えます。
- **Issues** - issue を参照してフィルターしたり、issue からセッションを開始したりできます。
- **Automations** — スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。ToDo リストの管理、定期的なプロジェクト保守、その他の面倒な作業に役立ちます。まとめでは次のステップとして紹介しますが、別の演習ではありません。
- **Customize** - MCP サーバー、プラグイン、スキルなどのコンポーネントを追加して Copilot app の機能を拡張します。Playwright MCP の設定に使います。
- **Chats** — ブランチや独自のワークスペースを必要としない、質問やブレインストーミング向けの簡易的な会話です。最初のチャットを開始すると表示されます。表示されない場合は **Sessions** の横にある **+** を選択し、最初のチャットを開始します。
- **Sessions** — リポジトリ フォルダーとエージェント セッションをまとめます。`tailspin-toys` などのリポジトリ名を選択するとリポジトリ ビューが開き、その下のセッションを選択すると会話に戻ります。各セッションは分離された独自のワークスペースで動くため、変更を競合させずに複数のセッションを実行できます。星評価を追加するレッスンで最初のセッションを開始します。

ワークショップを進めながら、ワークスペースを確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問しましょう。操作方法がわからない場合や、何かが可能かどうか知りたい場合は、Copilot に尋ねると案内してくれます。

### 用意されたバックログを確認する

バックログのないプロジェクトはほとんどなく、Tailspin Toys も例外ではありません。テンプレートからリポジトリを作成したときに用意されたバックログを確認しましょう。

1. サイドバーの **Sessions** で、個々のセッションではなく Tailspin Toys のリポジトリ フォルダーを選択します。その後、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. issue を選択して詳細を読みます。issue からエージェント セッションを開始することもできます。最初の簡単な変更を完了した後、フィルター issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧は自動的に絞り込まれ、Copilot app に追加したリポジトリの項目だけが表示されます。ほかのリポジトリの作業項目を見るには、そのリポジトリをアプリに追加してください。

## クイックチャットを試す

アプリの使い方に慣れるには、アプリ自体について質問するのが効果的です。その用途には **quick chat** が適しています。Quick chat ではブランチや worktree を作成せずに質問やブレインストーミングができます。セッションを必要としない、その場限りの質問に便利です。

1. サイドバーで **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを開始します。その後 **Chats** が表示されます。
2. アプリのセッションがどのように動作するかを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションが分離された独自の Git worktree で実行されるため、変更が競合することなく複数のエージェントを並行して実行できることがわかります。会話はいつでも続けられ、新しいチャットも開始できます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次の操作を学びました。

- アプリをインストールして GitHub にサインインする。
- GitHub リポジトリからプロジェクトを追加する。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つける。
- クイックチャットで、その場限りの質問をする。

次は[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app を使い始める][getting-started]
- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
