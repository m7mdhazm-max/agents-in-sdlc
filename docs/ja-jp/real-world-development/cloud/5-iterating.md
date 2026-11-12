---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot にいくつかのタスクを依頼しました。コードへのドキュメント追加、デザイン チームが反復できる関連ゲーム機能の作成、ハイ コントラスト モードとライト モードを含むアクセシビリティ機能の実装です。コードの変更を確認し、必要に応じて Copilot にフィードバックを伝えて改善してもらいましょう。

### シナリオ

何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに従います。チーム全員にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監視を受けずに作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みできるのは作業用のブランチに限られます。
- Cloud agent は GitHub Actions 内で動作し、作業用に分離された一時的な環境を作成します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の標準的な PR インターフェイスを使って確認します。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーへの外部呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエスト一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は作業が完了するまで待ちます。数分かかることがあるので、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc の doc comment など、新たに追加されたドキュメントを確認します。具体的な変更内容は状況によって異なります。

    変更を確認しながら、TSDoc の doc comment、TypeScript の規約、コメント ヘッダーを確認します。これらは演習 2 の最初に確認したカスタム命令ファイルに基づいています。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業する方法は、一方的なやりとりだけではありません。チーム メンバーに依頼するように、プルリクエストのコメントやコードへの行コメントで Copilot にメンションできます。Copilot はそのコメントを読み、対応するための新しいセッションを開始します。結果は確定的ではないため、必ず同じ結果になる具体的な依頼文は示せません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc の doc comment を追加する。
- Astro アプリの構成を説明する README を作成する。

1. 生成されたドキュメントへの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションします。上記の例や、コードベースに追加したいドキュメントについて別の案を使ってください。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が変更のために新しいセッションを開始する様子を確認します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチの名前、たとえば **copilot/fix-8** に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、GitHub Copilot cloud agent に追加の変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc の doc comment を追加したりできます (この issue はカスタム命令を更新する前に割り当てたことを思い出してください)。**Conversation** タブに新しいコメントを追加すれば、Copilot が読み取って新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタム エージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装をレビューするときは、次の点に注意します。
   - モードを切り替える UI コンポーネント。
   - 利用者の設定を保存する local storage の使い方。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)。
   - モード切り替えを管理する JavaScript / TypeScript コード。

6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードを切り替えます。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタム エージェントが、アクセシビリティのベスト プラクティスに従って機能を実装するよう Copilot を導いたことを確認してください。アクセシビリティ上の懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションして変更を依頼できます。

## 任意の演習 — 委任を続ける

Cloud agent は、バックログにある実際の作業を任せて別の作業に集中するときに役立ちます。いくつかの issue をリポジトリに作成して Copilot に割り当て、この方法を身につけましょう。次のような作業を依頼できます。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent の演習を完了しました。各レッスンを通じて、次の作業を行いました。

- **このリポジトリに含まれるカスタム命令ファイルを確認し**、cloud agent の出力にどう反映されるかを確認しました。
- **issue を GitHub Copilot cloud agent に割り当て**、環境を準備し、計画して、非同期に作業する様子を確認しました。
- **カスタム エージェントを作成して使用し**、ハイ コントラスト モードとライト モードを追加しました。
- **Copilot agents ページを mission control として使い**、アクセシビリティ セッションを監視して、実行中に指示を追加しました。
- **Copilot のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent の演習は以上です。引き続き学ぶ場合は、ほかの環境の演習も役立ちます。

- 🖥️ **[VS Code の演習](../../vscode/)** — IDE 内で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI の演習](../../cli/)** — Plan モードと Autopilot モード、agent skill、QA 用カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` 連携を使って、レビュー済みの変更をターミナルから 2 つ届けます。

自分のリポジトリでは、次の作業も試してみてください。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
