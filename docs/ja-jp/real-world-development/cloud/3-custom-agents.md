---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置いてエージェントを定義し、対象の作業を効果的に行うための指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に伝えます。たとえば、[WCAG][wcag] 準拠をチェックするアクセシビリティ エージェント、安全なコーディング規約に従うセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントを作成して、チームの知識を再利用できます。

カスタム エージェントは、プロジェクト内の `.github/agents` フォルダーか、グローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後にエージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept] には役割が重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を伝えます。簡潔に区別するなら、**カスタム エージェントは作業者、スキルは道具**です。

カスタム エージェントには専用のコンテキスト ウィンドウがあり、作業を進めるためにスキルやほかのエージェントを組み合わせて使えます。このラボのアクセシビリティ用カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエスト ワークフローのスキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの作業と同じように、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次の操作を行います。

- カスタム エージェントの定義方法を確認する。
- カスタム エージェントにタスクを割り当てる。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング サイトを使えるようにしたいと考えています。最近の利用者のフィードバックによると、ダーク テーマでは文字と背景のコントラストが十分でなく、読みにくいと感じる人がいます。このアクセシビリティ上の課題に対応するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装する必要があります。カスタム エージェントを使って、この機能を生成しましょう。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認して、Copilot をどのように導くかを理解しましょう。

Codespace に戻り、アクセシビリティ用カスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する基本的な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と誤り。

## issue を作成して割り当てる

Mission control は、この環境でエージェントを操作するための中心的な場所です。GitHub Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を出せます。まず、ハイ コントラスト モードの実装タスクを Copilot に割り当てましょう。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントの一覧で Accessibility が選択された、cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に合わせた専門的な AI アシスタントである [GitHub Copilot のカスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの知識や標準を再利用できる形にして、Copilot が作業するときの指針にできます。

次の概念を学びました。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方法。

Copilot がハイ コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して指示する][next-lesson]方法を確認します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
