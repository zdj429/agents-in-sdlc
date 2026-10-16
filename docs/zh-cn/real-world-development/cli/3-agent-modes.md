---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据议题实现筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一个小功能。更大的更改则需要更稳健的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助以正确方式构建正确功能。本课是多个遵循典型智能体开发流程的课程之一：从议题开始实现新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 在后续功能流程中，会继续使用同一对话和分支。实际工作中，不同文件类型通常会使用不同分支或 PR；这里为了聚焦核心概念而采用简化方式。

本课将学习如何：

- 从 GitHub 议题启动新的 Copilot 对话；
- 在 Plan 模式下明确需求；
- 使用 Autopilot 模式实现功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续实现此功能时，还会更新仓库指令、自定义现有 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了此功能，但合并类别等细节需要在编写代码前达成一致。你将使用 Plan 模式确定这些选择，再授权 Autopilot 按明确范围实现。

## 背景

在开发流程中引入 AI 编码智能体，并不会改变基本原则；这些原则反而更重要。大多数开发者的工作流程大致如下：

1. 从已提交的议题开始，明确要完成的工作。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查通过且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多与上述流程类似。

遵循标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式间切换：

- **Interactive**：你与智能体共同工作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并持续迭代，直至任务完成。

先使用 Plan 模式并审查计划，然后使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认仓库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

此时提示应以 `#7`（或类似编号）开头。使用 `#` 可将 GitHub 议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可在委托 Copilot 实现功能或执行任务前，确定采用的方案。处理复杂任务时，最好先花时间规划。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在前一步添加的议题引用后，输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，然后确定合适的实现方案。

3. Copilot 可能会询问筛选功能应如何工作。根据偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动查看计划。
6. 如果计划中的任何内容不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划编写并审查完毕后，就可以开始实现了。使用 Autopilot 让 Copilot 执行任务。

Autopilot 会让 Copilot 持续处理问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中含义相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，应像其他代码一样在合并前进行审查。现在审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能和测试。
3. 将结果与议题以及规划时的决定进行比较。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，如有失败，请让 Copilot 修复。

## 体验新功能

代码看起来不错，但是否能运行？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商筛选，以及计划中确定的组合筛选。
4. 确认重置和无结果状态符合议题要求和你的决定。
5. 返回 Codespace，让 Copilot 停止启动的开发服务器。

## 总结和后续步骤

你已使用不同对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题启动新的 Copilot 对话；
- 在 Plan 模式中明确需求；
- 使用 Autopilot 模式实现功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

接下来通过[自定义指令][next-lesson]深入了解代码生成方式，确保代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
