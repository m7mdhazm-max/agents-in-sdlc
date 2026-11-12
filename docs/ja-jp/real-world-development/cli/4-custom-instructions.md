---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要があるなら、その指針を Copilot が参照できるようにします。[instruction ファイル][instruction-files]は、作成するコードの内容だけでなく、構成方法も説明します。フィルター機能を実装したので、Copilot が使った命令を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの命令とパス別 instruction ファイルがエージェントに適用される方法を確認する。
- コーディング規約に従うよう instruction ファイルを更新する。
- instruction ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発時に守るべきガイドラインと要件があります。

- コメントには、コードの内容を繰り返すのではなく、意図や自明でない決定を記載します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc / JSDoc を記載します。`db` を注入可能な引数として受け取る場合は、その引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連するコードを変更したときはコメントも最新の状態にします。
- 既存のフォーマットと lint の指針を維持します。

instruction ファイルを使って、Copilot がこれらの開発方針に沿って作業できるようにします。

## Instruction ファイル

カスタム命令を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード生成につなげられます。好みの規約、ライブラリ、コードに含めたいコメントなどを指定できます。リポジトリ全体に適用する命令と、特定のタスクやファイルの種類を対象にする命令を作成できます。

instruction ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送られる単一の instruction ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関する指針を記載します。

> [!NOTE]
> ほかの形式の instruction ファイルとその対応状況は、環境によって異なります。特定の形式を使う前に、[カスタム命令の対応状況に関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには、すでに複数の instruction ファイルがあります。変更を加える前に内容を確認して、その効果を比べられるようにしましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングの指針を確認します。これらの命令は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、命令を適用するファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction ファイルや既存のプロジェクト ファイルへのリンクを確認します。複雑な instruction を小さな再利用可能ファイルに分け、コード生成の参考例を Copilot に示せます。

## チームの方針に合わせて instruction ファイルを更新する

すでにあるファイルは出発点として役立ちますが、まだ不足している内容があります。中核となる `copilot-instructions.md` を編集し、新しく生成される TypeScript に TSDoc コメントが含まれるようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. 次の箇条書きをセクションの最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

変更は自動的に保存されます。

## 更新した指針を適用する

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。更新した命令を保存したら、フィルターのコンテキストを保ったまま会話を終了して再開し、新しい指針を読み込ませます。

1. フィルターの会話がまだ開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に更新した指針を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが生成され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。具体的には、次の操作を行いました。

- リポジトリの命令とパス別 instruction ファイルがエージェントに適用される方法を確認した。
- コーディング規約に従うよう instruction ファイルを更新した。
- instruction ファイルがコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令の対応状況][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction ファイル集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
