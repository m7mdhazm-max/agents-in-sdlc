---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两节练习中，你让 Copilot cloud agent 执行了三个任务，帮助改善用户体验并添加功能。Cloud agent 虽然可以异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的 [agents 页面][agents-page]。在这个任务控制中心，可以查看所有已创建开放拉取请求 (PR) 的智能体任务、了解它们执行的操作，还可以引导仍在进行的会话。

本课将：

- 查看 agents 页面并监控 cloud agent 任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以同时添加浅色模式。既然正在更新网站样式并添加模式切换功能，将其一并实现就很合理。现在引导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到你分配给 Copilot 的任务，包括：
   - 更新代码库文档。
   - 为修改游戏生成 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作。可能包括：
   - 从存储库检出代码。
   - 创建 Copilot 的工作环境。
   - 设置 MCP 服务器。
   - 执行完成所分配任务的各种操作。

> [!NOTE]
> 具体显示的步骤因 Copilot 的工作状态和执行方式而异。

4. 还要查看右侧显示的拉取请求 (PR) 面板，以便监控 PR 和更改的文件。

## 引导 cloud agent

了解当前正在执行的任务后，现在请 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择与添加高对比度模式相关的会话。确切标题可能因 Copilot 使用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成环境设置并开始工作。出现与以下示例类似的消息时，就表示已开始。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot agents 页面中的 cloud agent 任务截图，突出显示“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！发送消息时，它正在执行的步骤完成后，会将新请求纳入工作流程。

这同样需要几分钟，可以借此机会回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心位置。在此任务控制中心，可以跟踪所有正在进行和已完成的任务、审查工作，也可以重定向正在运行的任务来调整范围或提供额外指导。

本课探索了以下内容：

- 使用任务控制中心和 agents 页面监控 cloud agent 任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
