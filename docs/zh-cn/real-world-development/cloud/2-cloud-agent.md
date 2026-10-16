---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺乏时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主完成更新代码和添加功能等任务。智能体完成工作后，会创建一个草稿 PR，供开发人员审查。这样既能将繁琐任务交由智能体处理、加快开发进程，也能让开发人员专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 分配议题。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初负责网站的承包商没有留下合适的文档，事实上，代码完全缺少文档。团队希望先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们希望每个游戏的详细信息页面都能显示相关游戏，即同一类别中的其他游戏，让用户可以继续浏览。暂时不需要精致的实现，只要能用于 UX 验收测试即可。此项任务目前是阻碍，但优先级低于其他议题。

这两项工作都很容易被推迟，适合交给 Copilot 云智能体异步处理。开发人员可以继续其他工作，稍后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。就像与开发人员协作一样，可以通过多种方式分配工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。任务进行期间或完成后，你都可以查看 Copilot 会话。建议方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 编写范围明确的指令

GitHub Copilot 并非魔法，也不能替代思考。即使看似简单的操作，深入分析后也会发现复杂性，因此[为 Copilot 云智能体明确界定任务范围][cloud-agent-best-practices]很重要。应将其视为 AI 结对编程伙伴：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此仓库中的自定义指令

前面的练习介绍了自定义指令及其对 Copilot 的指导作用。如果你已经完成自定义指令练习，就见过 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。在分配工作前，先只读查看仓库中已有的指令文件，之后便能观察它们的影响。

在 GitHub Web UI 中打开以下文件；如果 Codespace 已在运行，也可以在其中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 章节，特别是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将这些指令限定为适用于 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，请留意生成的拉取请求是否遵循 TSDoc 注释、TypeScript 规范和文件注释头要求；这些要求来自上述指令文件。之后的练习审查 PR 时还会再次提到它们。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要设置特定环境并运行脚本，以确保工作环境正常。将任务交给 Copilot 也一样；它会以类似软件工程师的方式执行工作。

云智能体使用 [GitHub Actions][github-actions] 环境。你可以在 `.github/workflows/copilot-setup-steps.yml` 中配置一个[专用设置工作流][setup-workflow]，在智能体开始工作前运行，以提供所需的开发工具和依赖项。本实验已预先配置好该工作流，以便顺利完成练习。它会提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及此单体 Astro 应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在仓库的“Actions”选项卡中手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为步骤设置所需的最低权限。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果设置步骤需要克隆仓库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆仓库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端／应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几个重点：

- 工作流只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行该作业。
- 注意 `workflow_dispatch` 触发条件：你可以在仓库 **Actions** 选项卡中手动运行工作流。这样可以在等待 Copilot 运行前验证工作流是否成功。

## 添加文档

大家都知道文档很重要，但许多项目的文档已经过时，或根本没有文档。这类技术债务常常无人处理，降低工作效率，也让代码维护和新成员入职更困难。Copilot 擅长创建文档，因此可以将这一议题交给云智能体，让它在后台生成所需文档。后续练习会审查它的工作。

1. 在新的浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 议题分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似于 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示。Copilot 正在创建环境（运行前面提到的工作流）、分析项目并确定处理问题的最佳方案。

13. 查看列表及 Copilot 将要完成的任务。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。云智能体的主要目标之一就是异步执行任务，让你腾出时间处理其他工作。接下来我们会利用这一点：将另一项任务分配给云智能体，同时着手为应用添加功能。

## 为游戏详细信息页面添加相关游戏区域

如前所述，GitHub Copilot 云智能体的一大优势是可以并行分配工作：你可以处理一组任务，同时让它处理另一组任务。为设计团队添加相关游戏区域也许不需要太久，但这段时间仍可用于其他工作。现在就把任务交给云智能体。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题的第一条评论中会出现 👀，表示 Copilot 正在处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理你的第二项请求。云智能体的工作方式与软件工程师类似，因此你不必一直监控它；等任务完成后再审查即可。接下来了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给云智能体，让它异步处理技术债务、新功能，或将代码从一种框架迁移到另一种框架。

本课介绍了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以转而学习如何创建和使用自定义智能体。[云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用前面模块中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
