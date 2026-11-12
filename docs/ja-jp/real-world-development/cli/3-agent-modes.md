---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で issue に基づくフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

プロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存の作業フローに沿って動作し、適切なものを適切な方法で構築できるようにします。このレッスンから、典型的なエージェント主導の開発プロセスをいくつかのレッスンにわたって進めます。issue を基に機能を作り、コードの妥当性と機能の動作を確認し、最終的にプロジェクトにマージします。

> [!NOTE]
> 機能開発を続ける間、同じ会話とブランチを使います。通常は作業対象ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中できるよう簡略化します。

このレッスンでは、次の操作を行います。

- GitHub issue から新しい Copilot の会話を開始する。
- Plan モードで要件を決める。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動で検証する。

この機能開発の続きでは、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの issue には機能の説明がありますが、カテゴリを組み合わせる方法など、コーディング前に合意する詳細があります。Plan モードで決定し、範囲を明確にした実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、より重要になります。多くの開発者は、次のような流れで作業しています。

1. 作業内容を説明した issue から始める。
2. 実装する内容を計画する。
3. コードを作成してレビューする。
4. テストを実行して検証する。
5. 新しい機能を手動で確認する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> チームや組織によって詳細は異なりますが、一般的には上記の流れを調整したものになります。

この標準的な進め方に従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューのプロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、次に Autopilot で実装します。

## issue から始める

フィルター機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を作成すると、機能の実装やタスクの実行を Copilot に依頼する前に、方針を決められます。複雑な作業では、時間をかけて計画を立てることをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した issue の参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot が計画を作成します。まずプロジェクトを調べ、その後に最適な方法を決めます。

3. 作業の途中で、フィルターの動作について質問されることがあります。希望に基づいて回答してください。正解は 1 つではありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 決めた内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューできました。次は Autopilot モードで実装します。

Autopilot を使うと、Copilot は完了したと判断するまで問題に取り組み、反復できます。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンに表示される同様の選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まることがあります。先に計画をレビューしてください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同様にマージする前にレビューします。コードを確認し、サイトを起動して問題がないか確認しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を issue と計画時に決めた内容と照らし合わせます。
4. コードのレビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードは問題なさそうですが、実際に動くでしょうか。前回と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリと発行元のフィルター、および組み合わせを試します。
4. リセットと結果なしのときの動作が、issue と計画に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを使い分けて、機能の作成とレビューを行いました。このレッスンでは、次の操作を行いました。

- GitHub issue から新しい Copilot の会話を開始した。
- Plan モードで要件を決めた。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動で検証した。

次は[カスタム命令を使って][next-lesson]コードの生成方法をさらに詳しく確認し、コードが文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
