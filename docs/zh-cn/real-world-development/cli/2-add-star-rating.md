---
title: "第 2 课 - 添加星级评分：快速完成的小功能"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始修改项目了。先从一个小改动开始：游戏数据中已经包含星级评分，但首页的游戏卡片尚未显示。你将让智能体显示评分、审查更改，并通过第一个拉取请求合并修改。

本课将学习如何：

- 在功能分支上启动专注于某项任务的 Copilot 对话；
- 让智能体对项目进行小幅修改；
- 使用 `/diff` 审查修改；
- 运行应用，在转发的浏览器中确认修改；
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中每款游戏都有星级评分，游戏详细信息页面已显示评分，但首页的游戏卡片只显示标题、类别、发行商和简介。作为热身，你将让智能体在每张卡片上显示现有评分。这是一项小而独立的更改，非常适合作为首次会话任务。

## 对话的组成

**对话**是你在 Copilot CLI 中处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的仓库和 Git 分支，而不是创建专用工作树。保存的对话可让你稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你的提示和智能体回复、智能体探索和编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

启动新对话来实现功能：

1. 返回 Codespace。
2. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到显示游戏详细信息的文件并编写必要代码。现在已使用 Copilot CLI 添加新功能。

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使改动很小也不例外。现在在 Copilot CLI 中查看更改：

1. 输入 `/diff` 并检查每个已更改文件。
2. 确认游戏卡片在评分存在时显示数值，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 查看 Copilot 运行的检查结果；如有失败，请让它修复。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此实际代码可能有所不同。审查功能行为，而不是要求实现与某个固定方案完全一致。

## 检查更改

不能只阅读代码就假设它能正常工作。让 Copilot 启动网站，然后通过 Codespaces 转发的浏览器检查更新后的用户界面（UI）。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot，并让它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已完成，现在创建拉取请求（PR），将新代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 在按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux）的同时，选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查已更改文件和检查结果。
5. 准备就绪后，选择 **Merge pull request** 并确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜，你已使用 GitHub Copilot CLI 发布第一项更改！你：

- 在功能分支上启动了专注于任务的 Copilot 对话；
- 指示智能体修改游戏卡片；
- 使用 `/diff` 审查更改；
- 运行应用，在转发的浏览器中确认星级评分；
- 创建并合并了第一个拉取请求。

接下来从筛选议题开始，使用 [Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
