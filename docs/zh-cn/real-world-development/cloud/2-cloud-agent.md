---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码、添加功能等任务。智能体完成工作后，会创建一个草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

本课将使用 Copilot cloud agent 探索以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些待处理的技术债务。最初负责创建网站的承包商没有为项目编写文档。首先，团队希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望提升游戏发现体验。他们希望在每款游戏的详情页显示相关游戏，即同一类别中的其他游戏，方便用户继续浏览。目前不要求实现完整精致的功能，只需要可用于 UX 验收测试的版本。这项工作目前不是优先事项，其他议题的优先级更高。

这两项任务都可能被不断推迟，适合交给 Copilot cloud agent。它可以异步处理任务，让开发人员先专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。就像与人类开发人员协作一样，你可以通过多种方式启动任务，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配议题后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 会话运行期间或完成后查看详情。解决方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确界定说明范围的重要性

GitHub Copilot 并非魔法——它无法替你省去思考过程。即使看似简单的操作，深入分析后也会发现复杂性，因此请[谨慎界定交给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。将它视为 AI 编程伙伴：分阶段工作、不断学习和尝试，并根据情况调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果已经完成自定义说明的实践练习，就会看到 `.github/copilot-instructions.md` 的效果；如果还没看过，现在可以快速查看。将工作分配给 Copilot cloud agent 前，先只读查看存储库中已有的说明文件，以便稍后观察它们的作用。

可以在存储库的 GitHub Web UI 中打开以下文件；如果已有正在运行的 codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将这些说明限定为适用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给 cloud agent 时，请留意拉取请求中的 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自上述说明文件。后续练习审查 PR 时还会再次介绍。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境就绪。将任务分配给 Copilot 时也一样，因为它会以类似软件工程师的方式工作。

Cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。你可以在 `.github/workflows/copilot-setup-steps.yml` 中配置[专用设置工作流][setup-workflow]，让它在开始工作前运行，以便访问所需的开发工具和依赖项。为简化练习流程并提供学习机会，我们已提前配置好该工作流。它会为单体 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发器。它允许你从存储库的 **Actions** 选项卡手动运行工作流，以便测试是否成功，无需等待 Copilot 启动。

## 添加文档

大家都知道文档很重要，但多数项目的信息已经过时，或者完全缺少文档。这类技术债务经常无人处理，会拖慢工作效率、增加代码库维护难度，也让新开发人员更难加入团队。幸运的是，Copilot 擅长编写文档，因此这是分配给 Copilot cloud agent 的理想任务。它会在后台生成所需文档，后续练习中再回来审查其成果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，你应该会看到 Copilot 创建了一个待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看 Copilot 即将完成的任务列表。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解现状、暂停思考并确定合适的方案，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来会利用这一特点，再给 Copilot cloud agent 分配一项任务，然后编写代码为应用添加功能。

## 在游戏详情页添加相关游戏区域

如前所述，GitHub Copilot cloud agent 的一个重要优点是可以拆分工作：你专注于一组任务时，它可以处理另一组。为设计团队添加相关游戏区域可能不会花太多时间，但也可以利用这段时间处理其他任务。现在将这项工作分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，你会看到议题中的第一条评论旁出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求！Copilot cloud agent 的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。接下来开始创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将议题分配给 cloud agent，让它异步处理。它可以帮助解决技术债务、创建新功能，或协助将代码迁移到其他框架。

本课探索了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

Cloud agent 正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并能读取此前模块中介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
