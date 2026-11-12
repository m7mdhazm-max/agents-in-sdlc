---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索管理上下文、选择模型、共享会话以及可选云端委托的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 也提供了许多斜杠命令。这些命令可以调用高级功能、查看内部信息并设置其他选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在再来探索一些实用命令。

本课将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索并行处理、工作树以及委托任务给 cloud agent 的可选命令。

## 场景

核心 CLI 工作流已完成。现在探索一些其他功能：管理上下文、切换模型、共享会话，以及选择是否将工作委托给 [Copilot cloud agent][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未运行，请在存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用情况，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近填满时，Copilot CLI 会自动压缩上下文。若要自行决定压缩时间，可使用 `/compact`。如果要切换到无关任务并重新开始对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，不同开发人员也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作并分享经验有助于每个人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 若要共享会话，请选择适合的目标，例如 `/share file`，将会话导出为本地 Markdown 文件。
3. 将导出内容发送给其他人或发布之前，请先审查。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队无意共享的存储库或对话内容。

## 可选：扩展并行工作或委托任务

核心研讨会已完成。Copilot CLI 还提供了一些适用于较大任务的命令：

- `/fleet`：将相互独立的子任务分配给子智能体并行执行。
- `/worktree`：为独立任务创建隔离的 Git 工作树。
- `/delegate`：将任务发送给 GitHub Copilot cloud agent，异步处理并可能创建拉取请求。

这些命令是可选的，因为它们可能创建其他工作树或远程任务。开始前先创建一个范围明确的新任务，并按常规流程审查结果。若想深入了解异步智能体工作，可继续学习 [cloud agent 研讨会][cloud-workshop]。

## 总结和后续步骤

Copilot CLI 的斜杠命令可用于配置 CLI、共享会话并查看其内部运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看了可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行处理、工作树和 cloud agent 委托的可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索！通过[回顾所学内容][next-lesson]并了解后续步骤，为本次学习收尾。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
