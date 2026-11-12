---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]让你可以为开发工作流中的特定任务或领域创建专门的 AI 助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，可以为 Copilot 提供聚焦的说明、最佳实践、编码模式和领域知识，从而引导它更高效地完成特定类型的工作。团队可以将专业经验固化为可复用的智能体，例如确保符合 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中进行全局定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面跟着定义智能体行为、专长和说明的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]在逻辑上有一些重叠。两者主要通过 Markdown 文件定义，也都会指导 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是执行工作的角色，而 **skills** 是工具。

自定义智能体有独立的上下文窗口，能够在工作中编排技能（甚至其他智能体）。在本练习中，无障碍自定义智能体会根据无障碍指南审查并更新网站；执行任务时，它可以调用 PR 工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。和 AI 的其他用途一样，需要反复测试和迭代，找到最适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保其众筹平台对所有用户均可访问，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。本练习将介绍如何：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

项目已为无障碍任务创建了一个自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

返回 codespace，打开终端，并从 `main` 创建新分支来处理无障碍功能（将练习 3 的筛选 PR 保持独立）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接着浏览后续部分，查看其中强调的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。你可以选择自定义智能体，让聊天会话遵循该智能体的说明和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的新对话。不同练习所需的模式和模型各不相同，会在相关步骤中说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选中 **Agent**，请选择它。

   ![Chat 视图中的智能体选择器示例截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（其中显示当前智能体，默认值为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会开始工作——它会探索代码库、提出更改并修改项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和可审查的差异。
6. 随着编辑内容加入，**Files changed** 指示器会更新，显示智能体修改的工作集。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际更改存储库中的内容，例如 Astro 组件、CSS 和相关测试。

下一练习中，你将审查并引导此项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域定制的专用 AI 助手。通过自定义智能体，可以将团队的专业知识和标准固化为可复用的智能体，引导 Copilot 更高效地完成特定类型的工作。

本课探索了以下内容：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

下一步，[监控并引导智能体的工作][next-lesson]——审查它生成的更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
