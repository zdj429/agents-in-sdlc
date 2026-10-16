---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，需要确认它在浏览器中能正常工作。无需自己点击浏览应用；可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将学习如何：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用它在浏览器中手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

## 什么是模型上下文协议（MCP）？

智能体模式能够访问编辑器之外的资源后，功能会更加强大。模型上下文协议（MCP）是 Copilot 实现这一点的方式，它是一种让智能体与外部工具和服务通信的标准。

![智能体模式内部工作方式示意图，以及其与上下文、LLM 和工具的交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，智能体可以实时访问最新信息（使用资源）并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器负责管理 AI 智能体与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每台 MCP 服务器代表一组可供智能体访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 仓库的 API，让智能体可以创建仓库、更新现有仓库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让智能体可以浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可提供不同的工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，便于发现并参与生态系统建设。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前应仔细审查源代码、验证发布者并考虑安全影响。只使用你信任的 MCP 服务器，谨慎授予访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 文件中应已为你配置 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 项由项目模板提供，因此 Copilot 才能在上一练习中读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用工具时你需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控制的浏览器，正适合用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 项，与 `github` 并列，文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会通过运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器；Codespace 中没有可显示窗口的桌面环境，因此需要使用此模式。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装浏览器。如果 Copilot 后续提示缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动它并询问你是否信任该服务器。

1. 在 `.vscode/mcp.json` 中，`playwright` 项上方会显示 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过，VS Code 会在下一步中首次需要服务器工具时启动它并提示你信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。无需自己点击应用检查工作，智能体就能打开页面、浏览、应用筛选并读取结果，然后总结观察。无需离开对话即可快速确认功能行为是否符合预期。

Playwright MCP 服务器通过页面的[无障碍树][playwright-mcp-server]工作，而非依赖截图。这意味着智能体可以根据结构化的带标签元素（按钮、链接、列表项）进行推理，方式与辅助技术类似，因此快速功能检查也能顺带进行基本无障碍检查。

服务器已连接且应用正在运行时，让 Copilot 测试刚刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步检查并报告结果。对照议题中的验收条件阅读总结；如果发现异常，可继续提问，或让 Copilot 修复代码后再创建拉取请求。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，即可创建拉取请求（PR）供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交说明。

   ![显示 Copilot 所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到仓库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建拉取请求。既然正在使用 GitHub Copilot，就让它来创建 PR。它可以找到相关议题并创建与议题关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示你进行 GitHub 身份验证；按照提示操作即可。
5. 观察 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接查看 PR，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 在发布前使用它通过浏览器手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

你已使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来[创建自定义智能体][next-lesson]，简化无障碍审查等特定任务。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
