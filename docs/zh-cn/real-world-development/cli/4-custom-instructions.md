---
title: "第 4 课 - 使用自定义指令指导 Copilot"
description: "查看仓库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，应确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将查看 Copilot 使用的指令、添加文档标准并将其应用于代码。

本课将学习如何：

- 查看仓库指令和按路径指定的指令如何传递给智能体；
- 更新指令文件，确保遵循编码标准；
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 制定了开发实践指南和要求：

- 注释应解释意图和不明显的决定，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值，并在适用时说明可注入的 `db` 参数。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码变更时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

借助指令文件，Copilot 可以按照这些实践完成任务。

## 指令文件

自定义指令可提供上下文和偏好设置，帮助 Copilot 理解编码风格和要求。你可以指定编码规范、库以及希望添加的注释类型，从而获得更相关的建议和代码片段。可为整个仓库创建指令，也可针对特定文件类型或任务创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是适用于 Copilot 对仓库**每次**请求的单一指令文件，应包含大多数请求都需要的项目级信息。
- `.github/instructions/*.instructions.md` 文件为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。使用特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 浏览此项目中的自定义指令文件

入门项目已包含一组指令文件。先查看现有文件，再进行修改，以便观察其效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。这些指令适用于在此仓库中与 Copilot 进行的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览文件。这里有针对 Astro 文件、Drizzle 数据层、测试等的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用 glob 模式指定适用文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目现有文件的链接。这种方式可以把指令拆分为更小、可复用的文件，并引用示例供 Copilot 参考。

## 更新指令文件以符合团队标准

现有文件是很好的起点，但仍有改进空间。修改核心 `copilot-instructions.md` 文件，要求新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 章节，通常位于第 35 行附近。
2. 在该章节最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在新对话开始时加载仓库指令。保存更新后的指令后，退出并恢复筛选对话。这样 Copilot 就能应用新指导，同时保留功能上下文。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的仓库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确解释了代码。

## 总结和后续步骤

你了解了 Copilot CLI 如何获取指令文件中的上下文，并将新标准应用于功能。你：

- 查看了仓库指令和按路径指定的指令如何传递给智能体；
- 更新指令文件以确保遵循编码标准；
- 了解了指令文件对代码的影响。

接下来[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试始终以一致方式运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
