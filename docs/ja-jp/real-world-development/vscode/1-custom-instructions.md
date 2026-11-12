---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。特に重要なのは、コード生成の指針を伝える[ instruction ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパス別 instruction ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認する。
- 現在の命令を使って、フィルター機能の最初のデータ部分 (発行元ヘルパー) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトをもう一度実行し、生成コードに新しい標準が反映されることを確認する。
- 次の演習で Copilot が更新した指針を使えるよう、命令の変更とフィルター機能の基礎を `main` にコミットする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確定的ではありません。この演習では、命令を更新した後に動作がどのように**変化するか**を確認します。同じ出力を完全に再現することが目的ではありません。

## Instruction ファイル

### シナリオ

Tailspin Toys には、開発時に守るべきガイドラインと要件があります。

- データ層には必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

instruction ファイルを使って、Copilot がこれらの開発方針に沿って作業できるようにします。

### カスタム命令

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード生成につなげられます。規約、ライブラリ、コードに含めるコメントなどを指定できます。リポジトリ全体に適用する命令と、特定のファイルやタスクを対象にする命令を作成できます。

instruction ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られる単一の instruction ファイルです。技術スタック、開発中の内容、ベスト プラクティス、全般的な指針など、ほとんどのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象にできます。TypeScript などの言語や、UI コンポーネントまたはユニット テストの作成などのタスクに関する指針を記載できます。

> [!NOTE]
> IDE では、instruction ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの形式の instruction ファイル][custom-instructions-support]にも対応しています。

### Instruction ファイルを管理するためのベスト プラクティス

instruction ファイルの作成方法をすべて説明することはこの演習の範囲外ですが、サンプル プロジェクトには代表的な方法を示しています。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構成、全体に適用するコーディング規約など、プロジェクト レベルの指針に絞って記載します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ層などのファイルの種類や、特定のタスクに関する指示に使います。
- 自然な言葉を使い、明確な指針とコードの例を示します。

instruction ファイルの作成方法に決まった形はありません。AI のほかの活用方法と同じように、試しながらプロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分に整備された instruction ファイル一式があると役立ちます。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction ファイルを作成してもらうこともできます。操作は環境によって異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。詳細は各環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、instruction ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに含まれる instruction ファイルを読んでみましょう。中核となる `copilot-instructions.md` が 1 つあり、さまざまなタスク向けの `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認してください。これらは Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する instruction ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象をリポジトリ ルートからの相対 glob で指定していることを確認します。この例では `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の instruction ファイルや、プロジェクト内の既存ファイルへのリンクがあります。複雑な命令を再利用可能な小さなファイルに分け、コード生成の参考例を Copilot に示せます。ここに記載されたパスはリポジトリ ルートではなく、instruction ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約がありますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベスト プラクティスに従い、作業用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

カスタム命令の効果を確認するため、まず現在の命令を適用した状態でプロンプトを送ります。その後、命令を更新して、同じプロンプトをもう一度送信したときの違いを確認します。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディター タブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選び、ファイルを編集できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認してコードを更新します。ヘルパー ファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーが、型付きの発行元配列を返し、最初の引数として `db` クライアントを受け取る関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードに TSDoc の doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する可能性があります。その場合も問題ありません。この演習では、命令を更新した後に動作が**一貫して改善する**ことを確認します。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクト全体の情報を伝えます。コードの提案を改善するために、リポジトリのコーディング規約を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトの規約が説明されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーに doc comment がありませんでした。
3. 既存の規約のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc comment を使うよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、言語、ファイルの種類、タスクごとに `*.instructions.md` として作成することもできます。今回追加した doc comment のルールのように、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 同じプロンプトをもう一度送り、変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使用する実際のコードに適用しました。

## 命令をコミットしてブランチをプッシュする

instruction ファイルもほかのリポジトリのファイルと同じように、ソース管理で管理します。変更をコミットしてブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルから次のコマンドを実行し、命令とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルから命令と Copilot がヘルパーの基礎を作成するために変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の issue に関連付けてプルリクエストを作成します。手動で行うこともできますが、GitHub ツールに接続済みの Copilot に issue を検索し、issue をクローズする PR を作成して、マージまで依頼できます。

> [!NOTE]
> この演習では、Copilot が初めて GitHub 上の操作を行います。プロジェクトのテンプレートでは、GitHub ツールがワークスペースにあらかじめ接続されています。セットアップは不要ですが、Copilot が初めてツールを使うときに VS Code から GitHub へのサインインを求められることがあります。画面の指示に従って許可してください。後の演習では、この接続の仕組み (Model Context Protocol) について説明します。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. instruction ファイルの更新に関する issue を検索し、現在のブランチから PR を作成して、命令の更新と発行元ヘルパーの基礎の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を検索して PR を作成します。
5. GitHub の操作を許可するよう Copilot に求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作とシェル コマンドの実行を許可するよう Copilot に求められたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が instruction ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 現在の instruction を使ったコード生成プロンプトを送信し、Copilot の出力を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加した。
- 同じプロンプトを再送信し、提案されたコードに新しい標準が反映されたことを確認した。
- 次の演習で使えるように、命令の更新とヘルパーの基礎を `main` にコミットした。

次は Copilot がコードベース全体に機能を追加するときに、[agent mode でこれらの命令を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — instruction ファイル集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
