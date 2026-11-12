---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、報告形式をカスタマイズして、フィルター機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業には、単にコードを書く以上のことが含まれます。手動でコードの動作を確認し、instruction ファイルを使って規約に従うようにしました。テスト、lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**agent skill** が適しています。スキルを使うと、こうした処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認する。
- 結果の報告形式をカスタマイズする。
- スキルを実行し、出力をレビューする。

## シナリオ

Tailspin Toys は、ユニット テスト、lint、型チェックに `quality-checks` スキルを使います。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 命令、スクリプト、リソース

Agent skill は、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースをまとめたもので、エージェントが必要に応じて読み込みます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` という Markdown ファイルで構成されます。Markdown には、スキルの名前と説明を定義する frontmatter、スキルの概要、呼び出すタイミングの指針が含まれます。フォルダーには、呼び出し時に使うスクリプトやリソースを含むサブフォルダーを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。この例では `npm` コマンドでテストと linter を実行するため、補助ファイルは必要ありません。

スキルをプロジェクトの `.github/skills` フォルダーに置くと、チームで共有・再利用できるリポジトリのアセットになります。また、Copilot のルート フォルダー (通常は `~/.copilot/skills`) に置くこともできます。

## スキルを確認する

Tailspin Toys チームがユニット テスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. **Files** キャンバスをまだ開いていない場合は、レビューパネルで **+** を選択し、次に **File** を選択します。
2. `.github/skills/quality-checks/SKILL.md` を検索します。
3. 冒頭の `name` と `description` を読みます。説明によって、Copilot がスキルを呼び出すタイミングを判断できることを確認します。
4. 指示を読み、テストと lint のプロセスを Copilot にどう案内するか確認します。

## 変更前にスキルを実行する

スキルはスラッシュ (`/`) コマンドで直接呼び出すか、自然言語で呼び出せます。スキルに 3 種類のチェックを実行してもらいましょう。

1. モードのドロップダウンから **Interactive** を選び、Copilot が Interactive モードであることを確認します。
2. 次のプロンプトでスキルを呼び出します。

    ```plaintext
    Run the quality-checks skill for unit tests, lint, and type checks.
    ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際に何を報告したかがわかる、よりよいレポートが必要です。レポート形式を改善するようスキルを更新しましょう。

1. **Files** キャンバスに戻ります。
2. まだ開いていない場合は `.github/skills/quality-checks/SKILL.md` を開きます。
3. ファイルの末尾に次のセクションを追加します。

    ```markdown
    ## Results output formatting

    Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
    ```

ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更したスキルを試すため、先ほどとまったく同じプロンプトを使います。

1. モードのドロップダウンから **Interactive** を選び、Copilot が Interactive モードであることを確認します。
2. 次のプロンプトでスキルを呼び出します。

    ```plaintext
    Run the quality-checks skill for unit tests, lint, and type checks.
    ```

3. 最後に表示されるレポートを確認します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使用しました。このレッスンでは、次の操作を行いました。

- ユニット テスト、lint、型チェック用の `quality-checks` スキルを確認した。
- 結果の報告形式をカスタマイズした。
- スキルを実行し、出力をレビューした。

この変更はフィルター機能と一緒に機能の PR に含めます。次は [Playwright MCP サーバーを通じて][next-lesson] Copilot にサイトを直接操作させます。

## ほかのスキルの例

コミュニティの次の例は参考資料であり、追加のタスクではありません。採用する前に前提条件と動作を確認してください。

- [Agent Skills 仕様][skill-spec]。
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]。
- [要件文書: `prd`][prd-example]。
- [図と同梱のエクスポート スクリプト: `drawio`][drawio-example]。
- [ブラウザー テスト: `webapp-testing`][browser-example]。

上流のコントリビューション例の名前は `make-repo-contribution` です。古い Tailspin テンプレートでは別の名前 `make-contribution` を使っていました。このワークショップは、どちらのコントリビューション スキルにも依存しません。

[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
