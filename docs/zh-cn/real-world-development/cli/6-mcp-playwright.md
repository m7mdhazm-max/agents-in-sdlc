---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面已经提到，编写代码不只是写代码。我们还要处理数据、外部服务，并让 Copilot 使用其他自动化工具。MCP 服务器可以扩展 Copilot CLI 内置的能力，为它提供更多工具和服务。

本课将：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚不可用，则添加该服务器。
- 请智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更改，还需要实际与 UI 交互。你可以让 Copilot 像用户一样操作正在开发的网站，进一步自动化更改验证流程，从而更有把握地确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问外部工具和服务，从而获取最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都提供一组不同的工具和资源，供 AI 智能体访问。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器可供使用。GitHub 托管了一个 [MCP 注册表][mcp-registry]，帮助用户发现并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请审查其源代码、验证发布者并评估安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器的对话框：

   ```plaintext
   /mcp add
   ```

3. 在名称字段中输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 将以下内容粘贴到 **Command** 对话框中：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 通过 Playwright 探索功能

之前你已手动确认功能符合预期。现在使用 Copilot CLI 中的 Playwright MCP 服务器执行同样的检查。

1. 使用以下提示，请 Copilot 使用 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 实际上无需明确要求 Copilot 使用 MCP 服务器，它通常会自行判断。但既然你知道要使用哪个工具，明确指出有助于获得更一致的结果，也能节省一些 token。

2. 观察 Copilot 列出它在浏览器中执行的操作，以确认功能是否正常。
3. 阅读报告，确认所有行为均符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，然后向你提供报告。

## 总结和后续步骤

恭喜！你已使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中探索功能。回顾一下，你：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚不可用时添加了它。
- 请智能体驱动浏览器并探索筛选功能。

下一步，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具结合起来，形成专门的质量保证流程。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [为 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
