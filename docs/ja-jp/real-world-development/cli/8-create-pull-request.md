---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。このワークショップを簡潔に進めるため、フィルターと品質ワークフローを 1 つの会話とブランチにまとめ、1 つの PR に含めています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学ぶ。
- 機能全体の変更と検証結果を確認する。
- フィルターの PR を作成する。
- レビューした後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能の作業では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys は、残りの PR 作業を自動化したいと考えています。ただし、マージを許可するかどうかは開発者が管理します。

## Agent Merge の概要

**Agent Merge** は、プルリクエストをマージできる状態にするための残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど) に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

ここまでは自分で **Merge pull request** を選択してきました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

すべてのコードを作成できたので、まとめてレビューして PR を作成し、残りの作業を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択して <kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行して PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot から `PR #14 was squash-merged successfully.` のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの処理など、開発プロセスの一部を自動化しました。具体的には、次の操作を行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学んだ。
- 機能全体の変更と検証結果を確認した。
- フィルターの PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意のクラウド委任に役立つ[その他の Copilot CLI スラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
