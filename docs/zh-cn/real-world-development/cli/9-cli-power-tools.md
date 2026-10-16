---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "了解用于管理上下文、选择模型、共享会话和可选委派到云端的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可用于访问高级功能、查看后台信息和调整配置。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在了解其他实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解如何通过 `/share` 导出或共享会话；
- 探索并行处理、工作树和委派给云智能体等可选命令。

## 场景

核心 CLI 工作流已完成。现在了解其他功能，包括管理上下文、切换模型、共享会话，以及可选地将任务委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型上下文窗口的最大限制。Copilot CLI 会在需要时自动压缩对话，也可以通过斜杠命令查看或手动压缩。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从仓库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌使用量，以及上下文在系统指令、工具、消息和可用空间中的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。如果希望自行选择压缩时机，请使用 `/compact`。切换到无关任务并需要全新对话时，可使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发人员也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和共享经验有助于大家更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中 `/share` 的选项。
2. 如果要共享会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前先进行审查。会话导出内容可能包含提示、回复和项目细节。

发布链接或 gist 是可选操作。不要发布团队不打算公开的仓库或会话内容。

## 可选：扩展处理规模或委派任务

核心研讨会到此结束。Copilot CLI 还提供了适用于大型任务的命令：

- `/fleet` 可将独立子任务分配给多个子智能体并行处理；
- `/worktree` 可为其他任务创建隔离的 Git 工作树；
- `/delegate` 可将任务交给 Copilot 云智能体异步处理，后者可能会创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或远程任务。尝试前请先创建范围明确的新任务，并按照常规流程审查结果。如果希望深入了解异步智能体工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

Copilot CLI 中的斜杠命令可用于调整配置、共享会话和查看后台运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文；
- 使用 `/model` 查看可用模型；
- 了解了如何通过 `/share` 导出或共享会话；
- 探索了并行处理、工作树和可选的云端委派命令。

Copilot CLI 还有更多斜杠命令和功能值得探索。接下来[回顾所学内容][next-lesson]并了解后续学习方向。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
