---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 协作完成了多项任务，以改善用户体验并添加功能：为代码添加文档、为设计团队实现可迭代改进的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。现在查看代码更改；如有需要，可向 Copilot 提供反馈以改进成果。

### 场景

如前面多次强调的那样，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队启用审查之前，先查看 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提供的更改。

## 安全性与 GitHub Copilot 云智能体

云智能体会在无人监督的情况下异步执行任务，因此设置了一些安全限制来保障安全：

- Copilot 对仓库只有读取权限，并且**仅**能对其用于工作的分支进行写入。
- 云智能体在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- GitHub Actions 工作流必须经人工批准才能运行。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

首先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），它为代码添加了文档。此任务通过 GitHub.com 上常用的 PR 界面完成。

> [!NOTE]
> 查看 PR 时，可能会看到提示 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或内容更详细的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成工作。可能需要几分钟，可以趁此休息一下，或回顾已学内容。

4. 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新添加的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，检查 TSDoc 文档注释、TypeScript 规范和文件注释头。这些要求来自练习 2 开始时查看的自定义指令文件。

6. 确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用的项目检查将会通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改内容

与 Copilot 协作并非单向沟通。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动新会话来处理反馈。由于结果具有非确定性，无法提供固定的提示文本。可以要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，说明 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用以上任一建议，也可以提出其他希望 Copilot 为代码库文档所做的修改。
2. 选择 **View Session**，查看 Copilot 执行修改的过程。注意 Copilot 会启动新会话来完成更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

你可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿状态改为准备审查，然后合并到主分支。

![将 PR 标记为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为“在游戏详细信息页面显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或内容更详细的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用的项目检查将会通过。此过程可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 中的终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已实现相关游戏功能！与之前一样，你可以与云智能体迭代协作并请求修改。例如，可以调整显示的相关游戏数量，或要求它添加注释头和 TSDoc 文档注释（请记住，分配此任务时还没有更新自定义指令）。同样，可以在 **Conversation** 选项卡中添加评论，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查无障碍自定义智能体实现的功能。此 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心要求添加的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或内容更详细的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现时，特别留意：
   - 用于切换模式的 UI 组件；
   - 是否使用本地存储保存用户偏好；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更改无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用的项目检查将会通过。此过程可能需要几分钟。
10. **可选：**你可以切换到 Codespace 中的此分支，手动测试无障碍功能。在终端中运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认设置可跨页面重新加载保留。

无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进空间，可以像之前一样在评论中提及 **@copilot**，请求修改。

## 可选练习 — 继续委派任务

云智能体最适合处理真实待办事项，让你把时间用于其他工作。可以在仓库中再创建几个议题并分配给 Copilot，培养这种工作习惯。可尝试以下任务：

- 在游戏详细信息页面创建支持者兴趣表单；
- 为游戏列表页面实现分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体环境的全部练习。在这些课程中，你：

- **检查了仓库自带的自定义指令文件**，了解这些指令对云智能体生成结果的影响；
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务；
- **创建并使用了自定义智能体**，实现高对比度和浅色模式切换；
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话；
- **审查并迭代改进了云智能体创建的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成云智能体环境的练习。如果希望继续探索，其他环境可以补充你刚才学到的内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 在 IDE 中直接体验 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的仓库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
