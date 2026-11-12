---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相应指导。[说明文件][instruction-files]不仅描述想要*什么*代码，也会说明代码应当*如何*组织。完成筛选功能后，你将探索 Copilot 使用的说明、添加文档标准并将其应用于代码。

本课将：

- 探索存储库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果有可注入的 `db` 参数，也要说明。
- 可复用 Astro 组件应说明其 `Props` 接口；代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 获取正确的信息，并按照这些实践要求完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。这是一项强大的功能，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定偏好的编码约定、库，甚至希望代码包含的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖特定格式前，请参阅[自定义说明支持参考][custom-instructions-support]。

## 查看本项目中的自定义说明文件

入门项目已包含一组说明文件。现在先查看现有文件，再修改内容并观察影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（而不是终端中）打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介和编码指导。此说明适用于 Copilot 在该存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段；它设置了一个用于确定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他说明文件和现有项目文件。这种方式可以拆分较大的说明集、复用较小文件，并向 Copilot 提供示例。

## 根据团队指导更新说明文件

现有说明是很好的起点，但仍有缺漏。现在修改核心文件 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 应用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认已检出 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并检查已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从说明文件中获取上下文，并将新的标准应用于功能。具体来说，你：

- 探索了存储库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

下一步，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
