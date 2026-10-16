---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备 Codespace、安装 Copilot CLI、探索项目，并找到已创建的筛选议题；
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求（PR）；
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下审查；
- 使用自定义指令指导智能体、自定义现有 `quality-checks` 技能，并使用该技能运行单元测试、lint 和类型检查；
- 添加 Playwright 模型上下文协议（MCP）服务器，并通过真实浏览器体验筛选功能；
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能结果和浏览器验证证据；
- 审查完整的筛选功能更改，并授权 Agent Merge 合并筛选 PR；
- 探索用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的功能

本研讨会包含两个 PR 里程碑，分别基于更新后的 `main` 分支创建：

1. **星级评分**：在游戏卡片上显示现有的 `starRating`，并明确标记尚未评分的游戏。
2. **筛选和质量工作流**：实现筛选功能、更新指令并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并包含相关测试。

从规划筛选功能到创建 PR，你都使用同一对话和分支。为简化研讨会，我们将相关工作合并到了一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、自己在浏览器中检查，以及 Copilot 通过 MCP 操作浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前结合这些结果，审查需求和测试覆盖率。

新增测试应弥补真实缺口；QA 运行时不需要添加测试也可能是正确结果。授权合并前，应审查代码和证据；更改后应重新生成受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。本研讨会中，你更新了指令、自定义技能、创建 QA 配置并设置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。指令用于设定标准，技能用于描述可重复的任务，自定义智能体用于定义专家角色，MCP 服务器则连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 在实现前确定方案，使用 **Interactive** 处理需要持续参与的重点更改，使用 **Autopilot** 执行范围明确的任务。常规编辑可选择更快的模型，复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清楚说明想要构建什么、为什么构建以及如何构建，会显著影响结果。

## 继续探索

你已完成核心工作流。还可以了解 Copilot CLI 的其他功能：

- 使用 `/review` 让代码审查智能体分析更改；
- 使用 `/rubber-duck` 讨论问题并获取另一种看法；
- 使用 `/fleet` 并行协调独立子任务；
- 使用 `/worktree` 隔离其他任务；
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

持续使用工具是提升技能的最佳方式。可以将其用于生产代码、个人项目，或用于实现一个想了多年却还没着手的小应用。与团队分享经验，也从团队成员那里学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。也可以探索其他环境：[VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
