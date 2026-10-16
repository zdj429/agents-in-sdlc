---
title: "第 8 课 - 创建并合并功能 PR"
description: "共同审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证（QA）配置和测试都保存在同一个分支中。现在将它们放在一起审查并创建拉取请求。之前你手动合并了星级评分 PR；这次让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 实际工作中，功能、指令更新、技能更新和 QA 智能体通常会拆分成几个独立 PR。为简化研讨会，我们将完整的筛选和质量流程保留在同一个对话和分支中，并通过此 PR 一并提交。

本课将学习如何：

- 了解 Agent Merge 及其如何自动处理合并流程；
- 检查完整功能更改和验证证据；
- 创建筛选功能 PR；
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的整个开发流程中，你使用 Copilot 规划、实现并验证功能。现在 Tailspin Toys 希望自动处理 PR 剩余工作，同时由开发人员掌控合并授权。

## 介绍 Agent Merge

**Agent Merge** 可自动处理 PR 合并所需的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复持续集成（CI）检查失败、回应审查评论以及必要时变基；之后，如果仓库设置允许，Copilot 会启用 GitHub 自动合并。

到目前为止，你一直是自己选择 **Merge pull request**。Agent Merge 可以接手这部分工作，但启用前仍需审查更改并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码已完成。现在共同审查代码、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成过程。一切成功后，它会执行合并。
7. 确认看到 Copilot 提示类似“PR #14 was squash-merged successfully.”的信息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、仓库设置或权限限制。如果流程被阻止，请阅读报告的原因，并在仓库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动化开发流程的多个部分，包括生成、测试和验证代码，以及处理拉取请求。你：

- 了解了 Agent Merge 及其如何自动处理合并流程；
- 检查了完整功能更改和验证证据；
- 创建了筛选功能 PR；
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来探索其他实用的 Copilot CLI [斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
