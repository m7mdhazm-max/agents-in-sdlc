---
title: "演習 6 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
| :-- |

## 作業をレビューする

このラボでは、ユーザー体験を改善するいくつかのタスクに GitHub Copilot と取り組んできました。agent mode でクライアントとサーバーの両方にフィルターを追加し、Playwright MCP サーバーでその作業をブラウザーから手動でテストしました。その後、カスタム エージェントを使って、高コントラスト モードとライト モードの切り替えというアクセシビリティ機能を実装し、進行中のセッションに指示を追加して作業を広げました。次は、そのローカルの作業を公開し、チームと同じ方法でレビューします。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。Copilot が生成するものには、引き続き通常のレビュー サイクルが必要です。Codespace からアクセシビリティの変更をプッシュし、プルリクエストを作成して、チームに参加してもらう前に差分を確認しましょう。

## アクセシビリティ機能を公開する

[演習 4][exercise-4] と [演習 5][exercise-5] でアクセシビリティ用カスタム エージェントを使って実装した、高コントラスト モードとライト モードの切り替えは、コミット済みの変更として Codespace にあります。チームがレビューできるように、ブランチをプッシュしてプルリクエストを作成しましょう。

1. Codespace に戻ります。
2. VS Code で **Source Control** ビューを開きます。
3. アクセシビリティの変更がコミット済みであることを確認します。演習 5 の未コミットの変更がある場合は、ステージして、`Add high-contrast and light-mode toggles` などの内容が分かるメッセージでコミットします。
4. **Publish Branch** を選択してブランチを公開します (または **...** メニュー → **Push** を選択します)。
5. VS Code から github.com で新しいブランチを開くよう促されたら、そのプロンプトを受け入れます。または、リポジトリに手動で移動して、ブランチのバナーにある **Compare & pull request** を選択します。
6. 分かりやすいタイトル (例: `Add high-contrast and light-mode toggles`) と、実施した内容と理由をまとめた簡潔な説明を入力します。
7. **Create pull request** を選択します。
8. PR が開いたら **Files changed** タブを選択し、作業全体をレビューします。特に次の点を確認します。
   - モードを切り替える UI コンポーネント。
   - 利用者の設定を保持する local storage の使い方。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript / TypeScript コード。

9. **Conversation** タブに戻ります。
10. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. ワークフローが完了するまで待ちます。問題がなければ成功します。

> [!TIP]
> アクセシビリティの作業について別の意見が必要ですか。PR コメントで `@copilot` をタグ付けし、「この PR に追加の WCAG の問題がないかレビューしてください」や「キーボード ナビゲーションの改善を提案してください」などと依頼します。Copilot はコメントに対応する新しいセッションを開始します。

## オプションの演習 - ローカルでさらに試す

IDE でエージェントと反復しながら作業することは、繰り返し練習して身に付けるスキルです。VS Code から実行できる追加セッションのアイデアを紹介します。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- `src/lib/` のデータアクセス ヘルパーに入力検証とエラー処理を追加する。
- アクセシビリティ エージェントの対象範囲を広げる。たとえば、サイト全体のキーボード フォーカス順を監査する。

## まとめ

おめでとうございます。VS Code の演習を完了しました。このラボでは、次の操作を行いました。

- **Playwright MCP を使って機能を手動テストしました。** Playwright MCP サーバーを追加し、プルリクエストを作成する前に Copilot にブラウザーを操作させてフィルター機能を検証しました。
- **agent mode でスタック全体にまたがる変更を連携して行いました。** 1 つのセッションで、クライアント、サーバー、テストにまたがるフィルター機能を追加しました。
- **カスタム エージェントを使いました。** エージェント選択メニューからアクセシビリティに特化したカスタム エージェントを選び、リポジトリに高コントラスト モードを実装する様子を確認しました。
- **エージェント セッションを管理して指示しました。** 提案された変更をインラインでレビューし、必要な変更を受け入れ、ライト モードの追加依頼でセッションを広げました。
- **プルリクエストで一連の作業を完了しました。** ローカルの作業を公開し、チームと同じ方法で全体をレビューしました。

## 振り返りと次のステップ

これで必須の VS Code 演習は終了です。ここでワークショップを完了してもかまいません。

Copilot のエージェント機能についてさらに学びたい場合は、ほかの演習で異なる環境から関連するシナリオに取り組めます。

- 💻 **[CLI の演習](../../cli/)** — Plan モードと Autopilot モード、スキル、QA 用カスタム エージェント、Playwright MCP、Agent Merge、コンテキストを踏まえた引き継ぎを使って、レビュー済みの変更をターミナルから 2 つ届けます。
- ☁️ **[Cloud agent の演習](../../cloud/)** — cloud agent への issue の割り当て、agents ページでのセッションの監視、プルリクエストでの非同期の反復に重点を置きます。

ここで始めた作業をさらに発展させることもできます。[awesome-copilot][awesome-copilot] は、自分のプロジェクトに応用できる instruction ファイル、カスタム エージェント、スキルを探すのに便利です。

オプションの拡張として、[オプション: Foundry を組み込む][exercise-7]では、VS Code と Microsoft Foundry Toolkit を使ってモデルを準備し、Backer Concierge をデプロイしてサイトに接続します。

## リソース

- [GitHub Copilot][github-copilot]
- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode の使用][agent-mode]

---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
