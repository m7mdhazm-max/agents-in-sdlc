---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本研讨会中，你已与 GitHub Copilot 合作完成了多项任务，以改善用户体验并添加功能。你让 Copilot 为代码添加文档、为设计团队构建相关游戏功能以供迭代，并实现了包括高对比度和浅色模式切换在内的无障碍功能。现在查看代码更改；必要时向 Copilot 提供反馈，进一步改进成果。

### 场景

如前面多次强调，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。基于这一点，在为团队其他成员启用审查前，先查看 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## 安全与 GitHub Copilot cloud agent

Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设置了一些安全限制以确保操作安全，包括：

- Copilot 仅对存储库具有读取权限，对代码则**仅**能写入它使用的分支。
- Cloud agent 在 GitHub Actions 中运行，并创建独立的临时环境来执行任务。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，其中为代码添加了文档。通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成。可能需要几分钟，可以休息片刻或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中应包含 TSDoc 文档注释和其他文档。具体更改可能有所不同。

   查看更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自练习 2 开始时查看的自定义说明文件。

6. 审查完更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的状态提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在拉取请求的检查列表中。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像 @ 团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，我们无法提供固定的请求文本。可以要求 Copilot 更新以下内容：

- 在每个代码文件开头添加注释头，简要说明文件的作用。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 在评论中使用 **@copilot** 提及 Copilot，请求它更改生成的文档。可以使用上面的建议，也可以提出其他文档改进请求。
2. 选择 **View Session**，观察 Copilot 执行修改。注意它会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回拉取请求。

   ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿状态改为就绪，然后合并到主分支。

![将 PR 转为可供审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 审查完更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的状态提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会出现在拉取请求的检查列表中。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能！和之前一样，可以通过迭代向 Copilot cloud agent 请求更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明）。和之前一样，可以在 **Conversation** 选项卡中添加评论来提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]**。此时请等待 Copilot 完成。这可能需要几分钟。

4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 审查实现，特别留意：
   - 用于切换模式的 UI 组件。
   - 如何使用本地存储保留用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完更新并确认符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的状态提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在拉取请求的检查列表中。如果一切正常，单体 Astro 应用的项目检查就会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 和终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认用户偏好在重新加载页面后仍然保留。

可以看到，无障碍自定义智能体帮助 Copilot 根据无障碍最佳实践实现这些功能。如果发现任何无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习——继续委托任务

当你可以把真实待办事项交给 cloud agent 并转而处理其他工作时，它的价值最大。可以在自己的存储库中创建更多议题并分配给 Copilot，逐步养成这种工作习惯。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页添加分页功能。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 学习路径。通过这些课程，你：

- **查看了存储库自带的自定义说明文件，**以便稍后观察其对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent，**并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义智能体，**为网站添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为任务控制中心，**监控并引导无障碍会话。
- **审查并迭代改进 cloud agent 的拉取请求，**通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 学习路径。若想继续探索，其他学习路径可以补充此处所学内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)**——直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)**——在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接功能。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
