---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、その他のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されているフィルター issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター issue から始め、Plan モードで方針を決め、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を試しました。
- 品質保証 (QA) 用のカスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルターの変更全体をレビューし、Agent Merge にフィルター PR のマージを許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには、それぞれ更新済みの `main` から作成した別々のブランチで進める、2 つの PR があります。

1. **星評価:** 既存の `starRating` と、評価がないことを明示する表示をゲーム カードに追加します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルターの計画から PR 作成までは、同じ会話とブランチを使いました。ワークショップを簡潔に進めるため、作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー操作、MCP を介した Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニット テスト、lint、型チェックを実行し、変更したレポート形式で結果を報告しました。QA はこれらの結果と要件、テスト範囲のレビューをまとめて PR の準備に役立てました。

追加するテストは、実際の不足を補うものであるべきです。QA の実行で新しいテストが不要と判断されても、問題ありません。マージを許可する前に、コードと検証結果を確認してください。変更があった場合は、影響を受ける検証結果を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し行う作業をまとめ、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前に方針を考えるため、**Interactive** は小さな変更を進めながら確認するため、**Autopilot** は範囲が明確なタスクを任せるために使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じように、コンテキストも重要です。何を作るか、なぜ必要か、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに確認する機能

基本的なワークフローを学びました。Copilot CLI には、ほかにも確認する価値のある機能があります。

- `/review` でコード レビュー エージェントに変更を分析してもらう。
- `/rubber-duck` で問題を整理し、別の視点から考える。
- `/fleet` で独立したサブタスクを並列に進める。
- `/worktree` で別のタスクを分離する。
- `/delegate` でタスクを Copilot cloud agent に送信する。

## 次のステップ

どのツールも、使い続けることで上達します。実際のコードにも、趣味のコードにも、長年作りたいと思いながら手をつけていなかった小さなアプリにも使ってください。学んだことをチームで共有し、チームの経験からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。別の環境も試すには、[VS Code のワークショップ][vscode]、[GitHub Copilot app のワークショップ][app]、または [Copilot cloud agent のワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
