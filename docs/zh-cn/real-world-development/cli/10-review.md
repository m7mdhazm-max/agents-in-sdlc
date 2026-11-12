---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已经在 Tailspin Toys 的完整工作流中使用 GitHub Copilot CLI。你完成了以下工作：

- 准备 Codespace、安装 Copilot CLI、了解项目并找到预置的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，并调整现有的 `quality-checks` 技能，用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中检查筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，检查需求、测试覆盖率、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云端委托的斜杠命令。

## 交付的内容

本研讨会有两个 PR 阶段，每个阶段都从更新后的 `main` 分支创建：

1. **星级评分：**在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选与质量工作流：**实现筛选、更新并应用说明、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建其 PR，你一直使用同一段对话和同一分支。我们将这些工作合并到一个 PR 中，以简化研讨会流程。

## 不同类型的验证

你通过自动化测试、亲自检查浏览器，以及使用 MCP 让 Copilot 探索浏览器来检查代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体在创建 PR 前汇总这些结果，同时审查需求和测试覆盖率。

新增测试应解决实际缺口；QA 运行后不需要添加测试也可能是正确结果。授权合并前，请审查代码和证据；更改代码后，也要更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义了技能、创建了 QA 配置文件并设置 MCP 服务器。可以在不同会话中复用这些自定义项，并根据团队需求调整。说明用于设定标准，技能描述可重复的任务，自定义智能体定义专门角色，MCP 服务器则连接外部工具。请审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 处理定义清晰的任务。常规编辑可选速度较快的模型；复杂工作则选择能力更强的模型。

上下文和基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出。

## 更多可探索的功能

以下 CLI 功能也值得了解：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取另一种看法。
- `/fleet`：并行协调相互独立的子任务。
- `/worktree`：隔离另一项任务。
- `/delegate`：将任务发送给 GitHub Copilot cloud agent。

## 后续步骤

提升工具使用能力的最好方法就是继续使用它！将它用于生产代码、个人项目，或那个想了多年却一直没开始编写的小应用。与团队分享经验，也向团队学习。一如既往，继续查阅文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。若想比较其他环境，可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot cloud agent 研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
