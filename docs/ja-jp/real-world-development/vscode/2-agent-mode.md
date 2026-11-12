---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイルを変更し、テストを実行するなど、さまざまな作業が必要になることがあります。開発者は通常、必要なファイルを探し、変更を行い、テストを実行し、問題をデバッグして、変更漏れがあるファイルを見つけて、さらに更新するという流れで作業します。作業は次々と続きます。

ここで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業します。開発者と同様に、既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる利用方法を想定しています。Agent mode は IDE 内で作業するため、すばやいフィードバックとやりとりができます。Cloud agent はチーム メンバーのように非同期で作業し、issue とプルリクエストを通じてやりとりします。

この演習では、次の操作を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行う。
- UI とデータ層にまたがる新機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。次は Copilot Agent Mode を使って、残りのデータ層、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを起動する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時に静的 HTML を生成します。ページは frontmatter 内で Drizzle ORM を介してローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` に置き、インタラクティブな動作には標準 DOM API を使った小規模な Astro `<script>` を使用します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習を終えて `main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行し、Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動転送されます。これは Codespace の Web サーバーに接続するためのプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細が表示されます。

## Copilot とバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトをセットアップすると issue のバックログも自動作成されています。GitHub Copilot に内容を表示してもらいましょう。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** はファイルをまたいで自律的に変更を計画・実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップの開発では Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案されるコードに一貫性を持たせ、学習しやすくするためです。ただし、LLM は確率的に動作するため、ワークショップの例とは異なる提案が表示されることがあります。これは通常の想定された動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログを表示するよう依頼します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が利用者にとって役立つと考えた順に、issue の優先順位も付けています。

## Instruction ファイルを確認する

エージェントにコードを生成してもらう前に、作業のコンテキストを伝える instruction ファイルを確認しましょう。[ユーザー インターフェイス (UI) instruction ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)には、Web サイトに機能を追加する方法に関するコンテキストがあります。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的な指針を確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などの各ファイル形式に関する instruction ファイルへのリンク。

> [!TIP]
> Instruction ファイルから、ほかの instruction ファイルやプロジェクト内のファイルを参照できます。パスは instruction ファイルの場所からの相対パスです。命令を再利用し、複雑な内容を小さな管理しやすい単位に分け、例やテンプレートを示せます。

## フィルター機能を実装する

フィルターを完成させるには、少なくとも 3 つの変更が必要です。

- データ層 (`src/lib/`) に残りのフィルター ロジックを追加または調整する。
- フィルターの動作を確認するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、instruction ファイルを指定します。

   ![UI instruction ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 次のプロンプトで、バックログにある関連 issue の要件に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データ層のヘルパー、UI、テストを見つけてから、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを確認する様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの編集、テストの実行などを行うため、タスクとコードベースの複雑さによっては数分かかります。処理中にコード エディターで **Keep** と **Undo** が表示されることがあります。作業が終わると、すべての変更に対して **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![テストの実行を確認する Copilot Chat のダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーが見つからなくなるまで作業します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを起動したブラウザーに戻り、新機能を試します。
11. 動作を確認し、コードのレビューが終わったら Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに機能を追加する方法を確認しました。

- UI とデータ層にまたがる新機能を実装した。
- プロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行った。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューした。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページ分割を実装する

ゲーム一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割の実装を依頼します。次の点を検討してください。

- 既存の instruction ファイルを使うなど、既存のベスト プラクティスに従います。
- ページ サイズを利用者が選択できるようにするか、固定にするかなど、ページ分割の方法を検討します。
- 希望する実装に必要な指示を Copilot に伝えるプロンプトを作成します。
- Copilot に変更を依頼したりコンテキストを追加したりして反復する必要があるかもしれません。Copilot を使う通常の流れです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの機能と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
