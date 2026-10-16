---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三项任务，用于改进用户体验并添加功能。云智能体可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有拥有未关闭拉取请求（PR）的智能体任务，了解它们执行了哪些操作，也可以引导正在进行的会话。

本课将介绍：

- 使用智能体页面监控云智能体任务；
- 在会话进行中引导智能体添加其他功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队决定同时添加浅色模式。网站样式和切换功能已经在更新，因此一并实现也很合适。你将引导智能体，让它在添加高对比度模式的同时也实现浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 留意主面板和左侧面板中的任务列表。你应能看到分配给 Copilot 的任务，包括：
    - 更新代码文档；
    - 为修改游戏生成 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从仓库检出代码；
    - 为 Copilot 创建工作环境；
    - 设置 MCP 服务器；
    - 执行完成所分配任务所需的各种步骤。

> [!NOTE]
> 具体步骤会因 Copilot 当前的工作状态和处理方式而异。

4. 同时留意右侧显示的拉取请求面板，可在此查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

现在你已了解活动任务，可以在智能体处理高对比度模式时要求它同时添加浅色模式。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到显示设置已完成并开始处理任务。看到类似下方的消息时，即表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，其中突出显示了引导正在运行会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务！收到消息时，它可能正在执行某个步骤；完成当前步骤后，它会将新请求纳入工作流程。

和之前一样，这需要几分钟。趁此机会停下来回顾一下目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心。在任务控制中心，你可以跟踪所有正在进行和已完成的任务、审查智能体正在执行的工作，还可以重定向正在进行的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 重定向正在进行的会话以请求添加其他功能。

Copilot 正在完成无障碍功能。接下来学习如何[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
