---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，组合使用议题要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并通过 Playwright MCP 在浏览器中体验筛选功能。现在将这些能力整合到自定义智能体中，定义明确的 QA 流程。

本课将学习如何：

- 了解自定义智能体如何配合指令、技能和 MCP 工具工作；
- 创建并检查可复用的质量保证配置；
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）之前，一致地审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本研讨会会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

已使用的自定义功能各有不同用途：仓库指令描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何使用这些能力评估需求并报告发现。它不会取代这些功能，也不需要启动另一个对话。

## 创建 QA 配置

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置。配置将定义 QA 执行的检查以及必须遵守的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，应检查其配置，确认 Copilot 记录了预期 QA 流程和限制。这样可以避免智能体在只要求验证时擅自修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 可选，但添加后会显示清晰名称。
3. 阅读配置说明，确认 QA 会先查看需求、遵循仓库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供证据、修改实现代码前会先询问，并且不会提交更改或创建拉取请求。
5. 如果配置遗漏任何职责或限制，请让默认智能体先修改，再继续操作。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置后，恢复同一个筛选对话并选择 QA，这样它就能使用已有的议题和规划决策。

1. 输入以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中，但仍可通过上述命令激活。

2. 使用以下提示让 QA 审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体将开始工作。
4. 完成后阅读它生成的报告。

## 总结和后续步骤

你已在工作流中添加可复用的专家角色，并审查了它的工作。本课中，你：

- 了解了自定义智能体如何配合指令、技能和 MCP 工具工作；
- 创建并检查了可复用的质量保证配置；
- 选择 QA 智能体，并根据筛选议题审查它的发现。

现在已有实现、技能更新、QA 配置、测试和验证报告。接下来将这些内容[合并到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
