---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要采用特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能获取这些上下文。[指令文件][instruction-files]可以提供指导，让 Copilot 不仅了解你要它做什么，也了解应该如何完成。

本练习将介绍：

- 如何通过仓库自定义指令和按路径指定的指令文件，为 Copilot 提供项目背景、编码指南和文档标准；
- 如何在 `.github/copilot-instructions.md` 中添加一项新的全仓库标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，此处不会使用前后提示进行对比。Copilot 云智能体会异步处理 GitHub 议题，因此很难实时展示前后差异。稍后审查云智能体创建的拉取请求时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 制定了一套开发实践指南和要求：

- 数据层必须始终配有单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应通过 TSDoc 文档注释添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 获得正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。你可以指定编码规范、库，以及希望添加的注释类型等，从而获得更相关的建议和代码片段。可以为整个仓库创建指令，也可以为特定文件类型或任务创建指令。

指令文件分为两种：

- `.github/copilot-instructions.md` 是一个指令文件，适用于 Copilot 对该仓库的**每次**请求。此文件应记录大多数聊天或 CLI 请求都需要的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可用于特定任务或文件类型。你可以创建适用于特定语言（如 TypeScript）的指令，也可以创建用于特定任务（如构建 UI 组件或编写单元测试）的指令。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用仓库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持 AGENTS.md、CLAUDE.md 等其他规范中的指令文件，详见[支持的自定义指令][custom-instructions-support]。

### 管理指令文件的最佳实践

本练习无法全面介绍如何创建指令文件，但示例项目展示了一种典型做法：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件、数据层）或任务提供具体指导。
- 使用自然语言，确保指导清晰，并提供代码示例。

创建指令文件并没有唯一正确的方法，使用 AI 也是如此。你可以通过实验找到最适合自己环境和场景的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有完善的指令文件集合。浏览本项目中的文件时，你可能会看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同使用界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关练习会在适用时介绍这些方式。
>
> 需要模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览此项目中的自定义指令文件

花点时间阅读此仓库自带的指令文件，其中包括一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。请在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简述以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。**Code standards** 下还包含 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件开头的 `applyTo` 字段，它通过 glob 模式（相对于仓库根目录）指定指令的适用范围。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 留意此项目中专门用于创建单元测试的指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。留意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可以将较大的指令集拆分成更小、可复用的文件，并引用示例供 Copilot 参考。（这些路径相对于指令文件，而非仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目编码标准，但尚未要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的仓库标准

接下来需要进行本练习中唯一的编辑操作：添加一项全项目规则，要求在代码中使用 TSDoc 文档注释，并在文件开头添加注释。之后的练习会分配议题给云智能体，它会遵循此规则。

编辑前，先创建一个分支（云智能体会读取议题目标分支中的指令，但你仍应规范地提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习将把议题分配给 Copilot，届时 Copilot 会从 `main` 分支创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求，并将其合并。下一练习中云智能体处理任务时就会读取这些指令。

> [!TIP]
> 如果你希望直接在 `main` 上完成本研讨会，也可以跳过分支操作并直接提交到 `main`。创建分支是为了让研讨会贴近真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何获取此项目的上下文，并在 `.github/copilot-instructions.md` 中添加了新的全仓库标准。之后的练习中，云智能体创建的拉取请求会展示这项标准的作用。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个提示][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
