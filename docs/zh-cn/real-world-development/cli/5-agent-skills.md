---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能，自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。我们已手动验证代码是否正常运行，并使用指令文件确保代码遵循标准。测试、lint 和其他持续集成（CI）工作又该如何处理？

**智能体技能**非常适合这类任务！技能能帮助 Copilot 正确执行这些操作。

本课将学习如何：

- 查看现有的 `quality-checks` 技能；
- 自定义技能的结果报告格式；
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 指令、脚本和资源

智能体技能将可复用的任务指令、可执行脚本和辅助资源打包在一起，供智能体按需加载。技能的基本结构是一个与技能同名的文件夹，其中包含 Markdown 文件 `SKILL.md`。此 Markdown 文件包含定义技能用途的 frontmatter（名称和描述）、功能概述，以及何时调用技能的指导。文件夹中还可以包含脚本和其他供技能运行时使用的资源子文件夹。

> [!NOTE]
> 技能不一定需要其他文件和文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为仓库资源供团队共享和复用；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 查看技能

查看 Tailspin Toys 团队创建的 `quality-checks` 技能。它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读开头的 `name` 和 `description`。描述有助于 Copilot 判断何时调用技能。
3. 阅读指令，了解它如何引导 Copilot 运行测试和 lint。
4. 注意技能尚未包含 **Results output formatting** 章节。

## 修改前先运行技能

Copilot CLI 支持直接调用技能，也可以通过自然语言调用。现在让 Copilot 运行技能中的三项检查。

1. 返回处于 Interactive 模式的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

希望报告能说明运行了哪些检查、检查是否成功，以及工具实际报告了什么。现在修改技能，让它生成这种报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下章节：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改已完成，现在查看效果。Copilot CLI 无需重启对话即可重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次使用之前的同一提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一次的报告比较。

## 总结和后续步骤

你已自定义并使用现有的智能体技能。本课中，你：

- 查看了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了结果报告格式；
- 重新加载并运行了技能。

筛选功能的 PR 会包含这项更改。接下来使用 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不属于本研讨会的额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
