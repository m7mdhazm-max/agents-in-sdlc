---
title: "练习 3 - 自定义智能体"
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

使用自定义智能体可以探索以下内容：

- 自定义智能体的定义方式。
- 如何向自定义智能体分配任务。

## 场景

Tailspin Toys 致力于确保其众筹平台对所有用户均可访问，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。接下来使用自定义智能体生成该功能。

## 查看无障碍自定义智能体

项目已为无障碍任务创建了一个自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

返回 codespace，然后查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接着浏览后续部分，查看其中强调的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误列表。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务，还可以重定向任务并提供更多指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![Copilot cloud agent 分配界面截图，突出显示自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域定制的专用 AI 助手。通过自定义智能体，可以将团队的专业知识和标准固化为可复用的智能体，引导 Copilot 更高效地完成特定类型的工作。

本课探索了以下内容：

- 自定义智能体的定义方式。
- 如何向自定义智能体分配任务。

Copilot 正在实现高对比度模式。接下来前往任务控制中心，[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织启用自定义智能体的准备工作][org-custom-agents]
- [为企业启用自定义智能体的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
