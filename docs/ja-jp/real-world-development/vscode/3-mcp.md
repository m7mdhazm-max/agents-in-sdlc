---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動くことを確認します。自分でアプリを操作する代わりに **Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にブラウザーで機能を手動テストするよう依頼する。
- ブランチを公開し、フィルターの変更に対するプルリクエストを作成する。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外にアクセスできるとさらに便利になります。Copilot がそのために使うのが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準的な方法です。

![agent mode の内部構成と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部サービスとリアルタイムに通信できます。これにより、リソースを使って最新情報にアクセスしたり、ツールを使って依頼に応じた操作を実行したりできます。

ツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーは、エージェントが利用できる別々のツールやリソースを提供します。

よく使われる MCP サーバーの例を次に示します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスします。AI エージェントに新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントに Web ページへの移動、フォームへの入力、ボタンの選択などを依頼できます。

さまざまなツールやリソースを利用できる MCP サーバーがほかにもあります。GitHub は、エコシステム内での発見と貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはほかの依存関係と同様に扱ってください。使用する前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように、`github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot がバックログの issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot が操作できるブラウザーが使えるため、機能のテストに役立ちます。

1. `.vscode/mcp.json` に `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモート サーバーの `github` とは異なり、`playwright` は**ローカル** サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面を表示せずにブラウザーを実行するよう Playwright に指示します。デスクトップ環境がない Codespace では、この指定が必要です。

> [!NOTE]
> Tailspin Toys ではすでにエンドツーエンド テストに Playwright を使っているため、Playwright が必要とするブラウザーは通常インストール済みです。Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行してからもう一度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot が初めてサーバーのツールを使うときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。選択してサーバーを今すぐ起動し、構成が正しいことを確認します。
2. サーバーを信頼するか尋ねられたら、構成を確認して、起動できるように信頼することを選択します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]が提供するブラウザーは、Copilot が操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を確認して、内容をまとめて報告します。会話を離れずに機能をすばやく確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使って操作します。つまり、エージェントは支援技術と同様に、ラベル付きの構造化された要素 (ボタン、リンク、リスト項目など) を基に判断します。そのため、機能の簡単な確認がアクセシビリティの初歩的なチェックにもなります。

サーバーを接続し、アプリを起動した状態で、Copilot にフィルター機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを介してブラウザーを起動し、各手順を実行して結果を報告します。報告内容を issue の受け入れ基準と照らし合わせます。問題がある場合は、質問を追加するか、PR を開く前にコードを修正するよう依頼します。

> [!NOTE]
> テストするにはアプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときは、ブラウザーをダウンロードする場合があります。ブラウザーが見つからないと報告されたら、`npx playwright install chromium` を実行してもう一度試すよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動くことを確認できたので、チームにレビューしてもらうためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを作成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、リポジトリにブランチをプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、プルリクエストの作成方法はいくつかあります。GitHub Copilot を使っているので、PR も作成してもらいましょう。Copilot は関連する issue を探し、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認し、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を集めたり操作を実行したりするため、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときに、GitHub への認証を求められることがあります。画面の指示に従って認証します。
5. Copilot が issue を検索し、該当する issue を見つけて PR を作成します。
6. Copilot が作成したリンクを選択して PR を確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- Playwright MCP サーバーでブラウザーからフィルター機能を手動テストした。
- ブランチを公開し、フィルター作業のプルリクエストを作成した。

MCP サーバーで機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティ レビューのような対象を絞ったタスクを効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
