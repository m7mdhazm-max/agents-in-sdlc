---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者有 Copilot 应了解的背景信息，就应确保它能够获取这些上下文。[说明文件][instruction-files]用于提供此类指导，帮助 Copilot 理解不仅要做*什么*，还要*如何*完成。

本练习将：

- 探索如何通过存储库自定义说明和按路径限定范围的说明文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本路径不会演示提示更新前后的对比。Copilot cloud agent 会在 GitHub 议题上异步工作，因此难以实时并排展示效果。后续练习中审查 cloud agent 创建的拉取请求时，你会看到说明文件的影响。

## 说明文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件的开头都应添加注释块，说明文件的作用。

通过说明文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。这是一项强大的功能，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望代码包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都会用到的上下文，例如技术栈、项目概况、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建。可为特定语言（如 TypeScript 或 Astro）提供指导，也可指导创建 UI 组件或一组单元测试等任务。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的做法。概括来说：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目概况、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码应如何编写以及不应如何编写的示例。

创建说明文件没有唯一正确的方法，使用 AI 也是如此。通过不断尝试，可以找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。查看本项目中的文件时，会发现其中涵盖了多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的实现方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。当前学习路径会在相关课程中介绍这些功能。
>
> 想找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中汇集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义说明文件

花些时间查看本存储库自带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意其中的项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览。这里有适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，以确定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都符合条件。
5. 查看为本项目编写单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 提供代码示例。这里的路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

本练习中唯一需要编辑的内容，就是添加一条项目级规则，要求代码中使用 TSDoc 文档注释，并在文件开头添加注释头。后续练习中，cloud agent 处理分配给它的议题时会遵循这条规则。

编辑前，先创建工作分支（cloud agent 会从议题目标分支读取说明，但仍建议干净地提交这些更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录存储库编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。注意该部分记录了项目编码标准，但尚未要求在代码中添加文档，这也是之前生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件既可放在存储库级文件 `.github/copilot-instructions.md` 中提供全局指导，也可使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。刚才添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

Cloud agent 会从议题目标分支读取说明。在下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将说明更改合并到 `main`，cloud agent 才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，cloud agent 处理分配给它的议题时就会读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从本项目的说明文件中获取上下文，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续练习中，你可以在 cloud agent 生成的拉取请求中看到该标准的作用。

下一步，[将第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
