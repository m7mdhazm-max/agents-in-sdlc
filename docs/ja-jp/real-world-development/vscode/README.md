---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** を使うと、普段のコード エディターで GitHub Copilot を利用できます。Visual Studio Code と GitHub Codespaces で Copilot Chat の agent mode を使い、MCP で外部ツールを接続し、カスタム エージェントを活用します。IDE を離れる必要はありません。Copilot はファイル、ターミナル、問題をすべて確認できます。

まずカスタム命令を追加し、Copilot がその命令に従う様子を確認します。次に agent mode を使って、UI、データ層、テストにまたがるフィルター機能を構築します。続いて Playwright MCP サーバーを接続し、プルリクエストを作成する前に Copilot にブラウザーを操作させて機能をテストします。最後にアクセシビリティ用のカスタム エージェントをレビューして使用し、エディターを離れずに Copilot の変更を監視、調整、反復します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | VS Code でカスタム命令を追加して確認する |
| [2. Agent mode][ex2] | コード生成 | agent mode でフィルター機能を構築する |
| [3. Playwright と MCP][ex3] | 外部ツール | Playwright MCP サーバーでブラウザーから機能をテストする |
| [4. カスタム エージェント][ex4] | 専門エージェント | カスタム エージェントを確認して使用する |
| [5. エージェントの管理][ex5] | 監視 | エージェント セッションを監視して指示する |
| [6. 反復][ex6] | レビュー | Copilot の作業をローカルでレビューし、次のステップを選ぶ |
| [オプション: Foundry を組み込む][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit でモデルを準備し、エージェントをデプロイしてサイトに接続する (全 3 モジュール) |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] GitHub Codespaces へのアクセス

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どの環境の演習も完了できます。

[callout-student-plan-education]: https://github.com/education/students

## はじめに

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
