---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务：未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成代码更新和功能添加等任务。任务完成后，它会创建草稿拉取请求（PR），供开发人员审查。这有助于将繁琐任务交给代理处理、加快开发流程，让开发人员专注于更宏观的工作。

你将探索如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的 issue。
- 将 issue 分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有为代码编写文档。首先，团队希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们希望每款游戏的详情页显示同一类别中的其他游戏，方便用户继续浏览。当前不需要精致的实现，只需要可以用于用户体验验收测试的版本。此需求目前受阻，不过还有优先级更高的 issue。

这两项任务很容易被推迟，很适合交给 Copilot cloud agent 异步处理。开发者可以继续处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub issue 分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。任务进行中或完成后，你都可以查看 Copilot 会话。提议的解决方案准备好等待审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的说明

GitHub Copilot 并非魔法，使用它并不能省去思考。看似简单的操作深入分析后也可能很复杂，因此请注意合理限定交给 Copilot cloud agent 的任务范围。把它当作 AI 结对编程伙伴：分阶段工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果你已完成自定义说明练习，就已了解 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配任务前，以只读方式查看存储库已有的说明文件，以便之后观察它们的影响。

在存储库的 GitHub 网页界面中打开以下文件；如果已有正在运行的 Codespace，也可在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` 前置内容，它将这些说明限定为 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` issue 分配给 cloud agent 时，请留意拉取请求中的 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述说明文件；后续练习审查 PR 时还会再次检查。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境就绪。将任务分配给 Copilot 时也一样，它会像软件工程师一样处理任务。

Copilot cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建专用设置工作流 `.github/workflows/copilot-setup-steps.yml`，在它开始工作前运行，以便安装所需开发工具和依赖项。本练习已预先配置此工作流，以简化流程并提供学习机会。对于这个单一 Astro 应用，它会为 Copilot 安装 Node.js、项目依赖项和端到端测试所需的浏览器，并准备迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

该文件与其他 GitHub 工作流类似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会执行此作业。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流。这样可以先测试工作流是否成功，而不必等待 Copilot 运行它。

## 添加文档

虽然大家都了解文档的重要性，但大多数项目的文档不是过时就是缺失。这类技术债务经常无人处理，会拖慢开发速度，增加维护代码库或帮助新开发人员加入团队的难度。Copilot 擅长创建文档，因此很适合将这类 issue 分配给 Copilot cloud agent。它会在后台生成所需文档，后续练习中你将回来审查结果。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 的待办列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方式。

13. 查看 Copilot 即将执行的任务列表。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理 issue 的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并决定合适的计划，以及编写代码。

此过程可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让你腾出时间处理其他工作。接下来利用这一特点：再分配一项任务给 cloud agent，同时开始编写应用功能代码。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组。即使为设计团队添加相关游戏区域不需要太多时间，委托后仍可以利用这段时间处理其他工作。现在将任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
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

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 issue 分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在 issue 的第一条评论中看到一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理 issue](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在处理第二个请求。Copilot cloud agent 的工作方式类似软件工程师，无需持续监控；等它完成后再审查即可。现在开始了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将 issue 分配给 cloud agent，让它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

你探索了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的 issue。
- 将 issue 分配给 Copilot。

cloud agent 正在后台工作，现在可以开始创建和使用自定义代理。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面练习中介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
