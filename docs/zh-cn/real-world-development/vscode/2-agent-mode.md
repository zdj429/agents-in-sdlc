---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行操作，例如运行测试。开发者通常要查找相关文件、修改代码、运行测试、调试、发现遗漏文件后再次修改……工作流程会不断重复。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可在 IDE 中更自主地工作，行为类似开发人员：先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 名称相似，但智能体模式和云智能体用于不同场景。智能体模式在 IDE 中执行任务，便于快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改；
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现功能；
- 合并前如何审查智能体模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望用户可以按类别和发行商筛选。上一练习中已添加发行商辅助函数，现在使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先了解 Tailspin Toys 网站当前的功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台，允许用户浏览游戏列表和详细信息。它是单体 Astro 应用，构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供 `npm run dev` 命令，用于启动单体 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示，询问是否打开 `http://localhost:4321` 浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通向 Codespace 的私有隧道，Web 服务器由 Codespace 托管。

### 浏览网站

网站运行后，可以浏览以下主要功能：

- **主页**：显示棋盘游戏列表，包括标题、图像和简介。
- **游戏详细信息页**：选择游戏后进入详情页，查看标题、简介、发行商和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异；相关步骤会在适用处说明。

网站的初始版本已能正常运行，但还可以添加功能。先查看待办事项。通过模板设置项目时，系统会自动创建议题列表；让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并非偏好某个模型。构建研讨会时使用了 Claude Sonnet 4.5，因此教程中采用该模型，希望代码建议相对一致，提供良好体验。但大型语言模型具有概率性，收到的建议可能与教程不同，这是正常且预期的情况。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，让 Copilot 查看议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 还会根据对用户的帮助程度为议题排序。

## 查看指令文件

让智能体生成代码前，先查看将用于提供工作上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中说明了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其中的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍原则；
   - 指向不同文件类型指令文件的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这有助于复用内容、拆分复杂指导，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用程序进行三项更新：

- 在数据层（`src/lib/`）中添加或完善其余筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页面，加入筛选 UI。

此外，合并前还需运行并通过测试。Copilot 智能体模式可以代为完成这些任务。

1. 可继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目、定位相关文件，包括数据层辅助函数、UI 和测试；随后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。具体耗时取决于任务复杂程度和代码库大小，可能需要几分钟。过程中代码编辑器可能出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 选项，因此无需在处理中逐个选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理中可能会有暂停，甚至看到测试失败，这没有问题。Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未发现错误。

   ![Copilot 智能体模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，发现潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 让智能体模式探索项目、识别相关文件并协调更改；
- 在合并前审查智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 — 实现分页

随着游戏列表不断扩大，需要启用分页。使用本练习中学到的技能，让 Copilot 更新网站并实现分页。可考虑：

- 遵循现有最佳实践，包括使用现有指令文件；
- 决定如何实现分页，例如允许用户选择每页数量，还是使用固定值；
- 在提示中提供 Copilot 所需的指导，以便按预期实现；
- 通过请求修改和提供上下文与 GitHub Copilot 迭代。这是与 Copilot 协作的常规流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
