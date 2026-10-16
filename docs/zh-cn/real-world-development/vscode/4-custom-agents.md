---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是专门面向开发工作流中特定任务或领域的 AI 助手。你可以在仓库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体，为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以把专业经验整理成可重复使用的智能体，例如负责落实 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护统一测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少要有 `name` 和 `description` 字段，后面是用于定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]在概念上有些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区别是：**自定义智能体是执行任务的工作者，技能是工具**。

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）来完成工作。本实验中的无障碍自定义智能体会依据无障碍指南审查并更新网站；执行过程中可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 创建自定义智能体没有唯一正确的方法。与 AI 的其他应用一样，需要通过测试和迭代找到适合自身环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于确保所有用户都能使用其众筹平台，不受视觉能力或偏好限制。最近收到的用户反馈指出，由于文字与背景颜色对比度不足，有些用户难以阅读当前的深色主题。为解决这一无障碍问题，设计团队要求实现一个可切换的高对比度模式。

无障碍性至关重要，因此希望尽快完成实现。你将使用自定义智能体生成所需功能。

本练习将学习如何：

- 查看现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

我们已为你创建一个无障碍自定义智能体。先查看它的内容，了解它将如何指导 Copilot。

返回 Codespace，在终端中从 `main` 创建新的无障碍功能分支（筛选 PR 将与本练习分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 查看后续章节，其中包括：
   - 生成无障碍网站代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 示例；
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 下定义的所有自定义智能体。选择自定义智能体后，此 Chat 会话会使用该智能体的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型因练习而异；相关步骤会在适用处说明。

1. 如果 Chat 视图智能体下拉列表中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（默认活动智能体为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每次编辑都会在聊天中以内联方式显示文件路径和差异，供你审查。
6. 智能体修改文件时，**Files changed** 指示器会更新，显示它修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会真正修改仓库，例如编辑 Astro 组件、CSS 和相关测试。

下一练习将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即专门面向特定任务和领域的 AI 助手。借助自定义智能体，可以将团队的专业经验和标准整理成可复用的智能体，从而更有效地指导 Copilot 完成特定类型的工作。

本课介绍了以下概念：

- 自定义智能体的定义方式；
- 在 Copilot Chat 智能体模式中使用自定义智能体。

接下来[监控并引导智能体的工作][next-lesson]，实时审查更改并要求它在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
