---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法で作業する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、その情報を参照できるようにします。[instruction ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」行うかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパス別 instruction ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の演習とは異なり、ここではプロンプトの「実行前と実行後」を比較しません。GitHub Copilot cloud agent は GitHub issue に対して非同期で作業するため、リアルタイムで並べて効果を示すのは困難です。instruction ファイルの影響は、この演習の後半で cloud agent が作成したプルリクエストをレビューするときに確認できます。

## Instruction ファイル

### シナリオ

Tailspin Toys には、開発時に守るべきガイドラインと要件があります。

- データ層には必ずユニット テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc の doc comment を使います。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

instruction ファイルを使って、Copilot がこれらの開発方針に沿って作業できるようにします。

### カスタム命令

カスタム命令を使うと、Copilot がコーディング スタイルや要件を理解できるように、コンテキストや好みを伝えられます。規約、ライブラリ、コードに含めるコメントの種類などを指定でき、より適切な提案やコード生成につながります。リポジトリ全体に適用する命令と、特定のファイルを対象にする命令を作成できます。

instruction ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られる単一の instruction ファイルです。技術スタック、開発中の内容、ベスト プラクティス、全般的な指針など、ほとんどのチャットや CLI リクエストに関係するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類を対象に作成できます。TypeScript などの言語や、UI コンポーネントやユニット テストの作成などのタスクに関する指針を記載できます。

> [!NOTE]
> IDE では、instruction ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの instruction と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの形式の instruction ファイル][custom-instructions-support]にも対応しています。

### Instruction ファイルを管理するためのベスト プラクティス

instruction ファイルの作成方法をすべて説明することはこの演習の範囲外ですが、サンプル プロジェクトには代表的な方法を示しています。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構成、全体に適用するコーディング規約など、プロジェクト レベルの情報に絞って記載します。
- `*.instructions.md` は、ユニット テスト、Astro コンポーネント、データ層などのファイルの種類や、特定のタスクに関する指示に使います。
- 自然な言葉で、明確な指針と例を示します。

instruction ファイルの使い方に決まった形はありません。AI のほかの活用方法と同じように、試しながらプロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分に整備された instruction ファイル一式があると役立ちます。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に instruction ファイルを作成してもらうこともできます。操作は利用する環境によって異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。詳細は各環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、instruction ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリに含まれる instruction ファイルを読んでみましょう。中核となる `copilot-instructions.md` が 1 つあり、さまざまなタスク向けの `*.instructions.md` ファイルもあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認してください。これらは Copilot に依頼する作業全般に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する instruction ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを指定する glob であることを確認します。この例では `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトでユニット テストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の instruction ファイルや、プロジェクト内の既存ファイルへのリンクがあります。複雑な instruction を小さな再利用可能ファイルに分け、コード生成の参考例を Copilot に示せます。ここに記載されたパスはリポジトリ ルートではなく、instruction ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング規約が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc の doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで編集するのは、TSDoc の doc comment とファイル レベルのコメント ヘッダーをコードに記載するという、プロジェクト全体のルールです。後の演習で issue を割り当てると、cloud agent はこのルールを参照します。

編集する前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチにある命令を読み取りますが、ここでは変更をきちんとコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えます。コードの提案を改善するために、リポジトリのコーディング規約を記載しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング規約が説明されていますが、コード内のドキュメントに関するルールがありません。そのため、生成されたヘルパーに doc comment がありませんでした。
3. 既存の規約のすぐ下に次の Markdown 行を追加し、ファイル コメント ヘッダーと TSDoc doc comment を使うよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、instruction ファイルはリポジトリ レベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、言語、ファイルの種類、タスクごとに `*.instructions.md` として作成することもできます。今回追加した doc comment のルールのように、プロジェクト全体の規約にはリポジトリ レベルのファイルが適しています。

## Instruction の変更をコミット、プッシュ、マージする

Cloud agent は issue の対象ブランチにある instruction ファイルを読み取ります。次の演習で Copilot に issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が新しい instruction を使えるように、変更を `main` に反映する必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開いてマージします。次の演習で issue に割り当てると、cloud agent はこの命令を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せずに `main` にコミットしてもかまいません。実際のプロジェクトに近い手順を体験できるよう、ここではブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が instruction ファイルからコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。この演習の残りで cloud agent が作成するプルリクエストで、この標準がどのように反映されるかを確認します。

次は [最初の issue を GitHub Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う instruction ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
