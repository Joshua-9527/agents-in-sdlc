---
title: "练习 2 - 使用 Copilot Agent Mode 添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也涉及多个文件以及运行测试等操作。开发人员需要找到所有相关文件、完成更改、运行测试、调试、发现遗漏的文件并继续修改，工作往往会不断重复。

Copilot Agent Mode 正是为此设计的。

Copilot Agent Mode 可以在集成开发环境（IDE）中更自主地工作。它会先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用 Agent Mode 为网站添加功能。

> [!NOTE]
> Agent mode 和 cloud agent 名称相似，但适用于不同的工作方式。Agent mode 在 IDE 中执行任务，便于快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过 issue 和拉取请求与你协作。

在本练习中，你将了解如何：

- 使用 Copilot Agent Mode 探索项目、找到相关文件并协调多文件更改。
- 使用 GitHub Copilot Agent Mode 跨用户界面（UI）和数据层实现新功能。
- 在合并到代码库前审查 Copilot Agent Mode 生成的更改和测试。

## 场景

游戏列表不断增长，你希望用户可以按类别和发行商筛选。上一练习中已添加发行商 helper，现在将使用 Copilot Agent Mode 完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先查看 Tailspin Toys 网站当前的功能，再进行修改。

这是一个面向开发者的桌游众筹平台，可以列出游戏并显示游戏详情。网站是单个 Astro 应用，在构建时将页面渲染为静态超文本标记语言（HTML）。页面在 frontmatter 中通过 Drizzle ORM 直接查询本地 SQLite 数据库；没有单独的后端应用程序编程接口（API）或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能由使用标准文档对象模型（DOM）API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321`，请通过选择 **x** 关闭它。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost 链接会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，网站服务器正在该 Codespace 中运行。

### 探索网站

网站运行后，可以探索其功能。主要页面包括：

- **主页**：显示桌游列表，包括标题、图像和描述。
- **游戏详情页**：选择某款游戏后，会打开包含更多信息的详情页，例如标题、描述、发行商和类别。

## 使用 Copilot 查看待办列表

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；每个步骤会在适用时说明。

网站初始版本已具备基本功能，但我们还希望添加新能力。先查看待办列表。根据模板设置项目时，系统已自动创建 issue；请让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某一种模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望代码建议相对一致、体验更顺畅。不过，大型语言模型（LLM）的输出具有概率性，你可能会看到与文档描述不同的建议。这完全正常。

> [!NOTE]
> 由于结果具有概率性，Copilot 可能会使用不同的模型上下文协议（MCP）命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，查看待办 issue：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有 issue 的命令。
5. 查看生成的 issue 列表。

留意 Copilot 还根据它判断的用户价值为 issue 排定了优先级。

## 审查说明文件

让代理生成代码前，先查看将用于提供上下文的说明文件。本练习将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)文件，其中说明了向网站添加功能时应采用的方法。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的整体指南，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向多种文件类型专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置。这样便于重复使用内容、拆分复杂说明并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三项更改：

- 添加或完善数据层（`src/lib/`）中的剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

合并前还需要运行并通过测试。Copilot Agent Mode 可以执行这些任务，现在请它添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办列表中的相关 issue 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。你应能看到它检查数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。工作完成后，每项更改都会显示 **Keep** 或 **Undo**，无需在处理中逐项选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中提示确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 处理中可能会暂停，测试也可能暂时失败。这没有关系！Copilot 会在生成代码和运行测试之间反复迭代，直到任务完成且未检测到错误。

   ![Copilot Agent Mode 完整 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加了功能。你了解了如何：

- 使用 Copilot Agent Mode 探索项目、找到相关文件并协调多文件更改。
- 使用 GitHub Copilot Agent Mode 跨用户界面（UI）和数据层实现新功能。
- 在合并到代码库前审查 Copilot Agent Mode 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习——实现分页

游戏列表不断增长后需要支持分页。使用本练习学到的技能，请 Copilot 更新网站以实现分页。编写提示时可考虑：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 确定分页方式：允许用户选择每页大小，还是使用固定值。
- 提示中提供 Copilot 所需的指导，确保实现符合预期。
- 你可能需要与 GitHub Copilot 多轮迭代，请它修改并提供上下文。这是正常的协作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：用途及适用时机][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
