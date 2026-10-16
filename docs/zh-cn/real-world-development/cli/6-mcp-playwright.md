---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不仅仅是写代码。我们还需要处理数据、外部服务，并让 Copilot 使用其他自动化工具。这正是 MCP 服务器的用途。MCP 服务器为 Copilot 提供 CLI 内置功能之外的工具和服务。

本课将学习如何：

- 了解模型上下文协议（MCP）及 Copilot CLI 如何使用它；
- 在尚未配置时添加 Playwright MCP 服务器；
- 让智能体驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要与 UI 实际交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改流程，并更有把握地确认更新符合预期。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具、服务之间的桥梁，每台服务器代表一组可供智能体使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 仓库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 托管了 [MCP 注册表][mcp-registry]，便于发现并参与生态系统建设。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已经配置 Playwright MCP，请跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你已手动确认功能是否正常。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示，让 Copilot 使用 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它一般可以自行判断。但你已知道应该使用哪个工具，因此明确指出有助于获得更一致的结果，也能节省一些令牌。

2. 观察 Copilot 在浏览器中执行的步骤，确认功能是否正常。
3. 阅读报告，确保行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器并提供报告。

## 总结和后续步骤

恭喜，你已使用 Playwright MCP 服务器，从 Copilot CLI 在真实浏览器中体验功能。回顾一下，你：

- 了解了模型上下文协议（MCP）及 Copilot CLI 如何使用它；
- 在尚未配置时添加了 Playwright MCP 服务器；
- 让智能体驱动浏览器并体验筛选功能。

接下来创建一个 [QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专门的角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
