---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 需要了解背景信息，就应确保它能获取这些内容。可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种是[指令文件][instruction-files]，用于指导代码生成。

本练习将介绍：

- 如何通过仓库自定义指令和按路径指定的指令文件，为 Copilot 提供项目背景、编码指南和文档标准；
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一项新的全仓库标准；
- 重新运行同一提示，观察生成代码如何采用新标准；
- 提交指令更新和筛选功能切片，并合并到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合设置的某些标准，因为 Copilot 的结果具有非确定性。本练习的目的是观察更新指令后的**整体行为变化**，而不是要求输出逐字完全一致。

## 指令文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应通过 TSDoc 文档注释添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。你可以指定编码规范、库以及希望添加的注释类型，从而获得更相关的建议和代码片段。可以为整个仓库创建指令，也可以为特定文件类型或任务创建指令。

指令文件分为两种：

- `.github/copilot-instructions.md` 是一个指令文件，适用于 Copilot 对该仓库的**每次**请求。此文件应记录大多数聊天或 CLI 请求都需要的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可用于特定任务或文件类型，例如针对 TypeScript 或 Astro 的指令，或用于创建 UI 组件和单元测试的指令。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用仓库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持 AGENTS.md、CLAUDE.md 等其他规范中的指令，详见[支持的自定义指令][custom-instructions-support]。

### 管理指令文件的最佳实践

本练习无法全面介绍如何创建指令文件，但示例项目展示了一种典型做法：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体指导。
- 使用自然语言，确保指导清晰，并提供代码示例。

创建指令文件并没有唯一正确的方法，使用 AI 也是如此。通过实验找到适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的指令文件集合。浏览本项目时，你会看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面的操作方式各异，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前环境的相关课程会介绍适用方法。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览此项目中的自定义指令文件

花点时间阅读此仓库自带的指令文件，包括核心 `copilot-instructions.md` 文件和一组适用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简述以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。**Code standards** 下还包含 **GitHub Actions Workflows** 指南。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。这里有针对 Astro 文件、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件开头的 `applyTo` 字段，它通过相对于仓库根目录的 glob 模式指定适用范围。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会符合此模式。
5. 留意项目中专门用于创建单元测试的指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可以把较大的指令集拆分成更小、可复用的文件，并引用示例供 Copilot 参考。（这些路径相对于指令文件，而非仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码标准，但尚未要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异；相关步骤会在适用处说明。

先使用当前指令文件发送提示，了解自定义指令的影响。稍后更新指令后，再发送同一提示进行比较。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是类型化函数，以 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 注意建议代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；本练习关注的是更新指令后行为的一致性改善。

## 添加新的仓库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以提升代码建议的质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，通常位于第 27 行附近。注意其中记录了项目编码标准，但尚无代码文档规则，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，指令文件既可以放在仓库级别（`.github/copilot-instructions.md`）以提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。仓库级文件适合存放全项目标准，例如刚添加的文档注释规则。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦于正确文件。
3. 重新发送之前的**同一提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议文件现在以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚让 Copilot 遵循新的项目标准，并将其应用到真实代码中。下一练习将在此基础上继续工作。

## 提交指令并推送分支

指令文件与仓库中的其他资源一样，都需要通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认指令更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础实现修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示有其他辅助更改（例如测试），也请将这些文件添加进去。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并将其关联到待办事项中的文档标准议题。也可以手动操作，但已连接项目的 GitHub 工具可以让 Copilot 代劳。让 Copilot 找到议题、创建并关联关闭该议题的 PR，然后合并它。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用 GitHub 工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录即可。后续课程会介绍这种连接的工作原理（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础实现：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当 Copilot 提示是否 **Allow** 它代表你执行 GitHub 操作时，审查命令并按需选择 **Allow**。
6. PR 创建后，使用以下提示让 Copilot 合并 PR 并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示是否允许它执行 GitHub 操作和 shell 命令时，审查命令并按需选择 **Allow**。

现在你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何获取此项目的指令文件上下文，并通过 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察现有指令下 Copilot 的输出；
- 在 `.github/copilot-instructions.md` 中添加新的全仓库标准；
- 重新发送相同提示，观察建议代码如何采用新标准；
- 提交指令更新和辅助函数基础实现，并合并到 `main`，为下一练习做好准备。

接下来进入[智能体模式][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
