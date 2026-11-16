---
title: "第 7 课 - 创建并使用 QA 代理"
description: "创建一个 QA 自定义代理，将 issue 需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中查看筛选功能。现在将这些能力整合到自定义代理中，定义清晰的质量保证（QA）流程。

在本课中，你将：

- 探索自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 代理，根据筛选 issue 审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地检查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程并生成可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。在本工作坊中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用过的自定义功能各司其职：存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则告诉 Copilot 如何利用这些能力评估需求并报告发现。它们无需彼此替代，也不需要开启另一个对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。该配置将同时定义 QA 执行的检查和必须遵守的边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 请默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理前，先确认 Copilot 捕获了预期的 QA 流程和边界。这样可以避免代理在你只想验证功能时，生成不完整的配置或擅自修改功能。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读前置内容。`description` 为必需项；`name` 为可选项，但添加后代理会有明确的显示名称。
3. 阅读配置说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，而且不会提交更改或创建拉取请求。
5. 如果生成的配置缺少上述职责或边界，请默认代理先修改，再继续。

## 根据 issue 运行 QA

Copilot CLI 会在对话启动时加载项目代理。创建配置后，恢复同一筛选对话并选择 QA，以便它使用已有的 issue 和规划决定。

1. 使用以下提示启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的代理可能暂时不会出现在可用代理列表中。仍可通过上述命令启用它。

2. 使用以下提示，请 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 代理执行任务。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已将可复用的专家角色加入工作流，并审查了它的工作。在本课中，你：

- 探索了自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 代理，根据筛选 issue 审查了它的发现。

现在，功能实现、技能更新、QA 配置、测试和验证报告均已准备好等待审查。接下来，[汇总这些更改并创建功能 PR，再使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
