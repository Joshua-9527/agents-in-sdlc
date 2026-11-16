---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本工作坊中，你与 GitHub Copilot 合作完成了多项任务，以改进用户体验并添加功能。你请 Copilot 为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的无障碍功能。现在检查代码更改；必要时向 Copilot 提供反馈，进一步改进结果。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队其他成员启用审查前，先检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提供的建议。

## 安全性和 GitHub Copilot cloud agent

Copilot cloud agent 会异步且无人监督地执行任务，因此设有一些安全限制：

- Copilot 对存储库只有读取权限，对其工作分支才有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先通过 GitHub.com 上的标准拉取请求（PR）界面，查看 GitHub Copilot cloud agent 创建的第一个 PR，也就是为代码添加文档的 PR。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**：Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果愿意，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 查看拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]**（work in progress，即“进行中”）标记。此时请等待 Copilot 完成。可能需要几分钟，可以休息一下或回顾迄今为止学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加了 TSDoc 文档注释和其他文档的代码。具体更改会有所不同。

   检查更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自练习 2 开始时查看的自定义说明文件。

6. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应出现在 PR 的检查区域中并进入队列。如果一切正常，针对单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 合作并非单向交流。你可以像在 PR 中提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新会话处理反馈。由于结果具有非确定性，我们无法规定必须使用的具体文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论，请求 Copilot 修改生成的文档，并像提及用户一样标记 **@copilot**。可以采用上面的建议，也可以提出其他文档改进意见。
2. 选择 **View Session**，观察 Copilot 如何执行修改。Copilot 会启动新会话来更新内容。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。完成后，可将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏” issue 创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应出现在 PR 的检查区域中并进入队列。如果一切正常，针对单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。和之前一样，可以通过后续请求迭代优化 cloud agent 的工作。例如，可以调整显示的相关游戏数量，或要求它添加文件注释头和 TSDoc 文档注释（注意：分配此任务时，你还没有更新自定义说明）。和之前一样，可在 **Conversation** 选项卡中添加评论，Copilot 会收到评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在 mission control 中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]**（work in progress，即“进行中”）标记。此时请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的用户界面（UI）组件。
   - 使用本地存储保留用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍富互联网应用（ARIA）属性、键盘导航等。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查完成且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应出现在 PR 的检查区域中并进入队列。如果一切正常，针对单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认刷新页面后设置仍然有效。

留意无障碍自定义代理如何引导 Copilot 遵循无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习——继续委托任务

Cloud agent 最适合处理真实待办任务，让你腾出时间做其他工作。可在存储库中创建更多 issue 并分配给 Copilot，以养成这种工作习惯。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页添加分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 环境的练习。在这些课程中，你：

- **检查了存储库自带的自定义说明文件**，以便之后观察其对 cloud agent 输出的影响。
- **将 issue 分配给 Copilot cloud agent**，并观察它如何设置环境、规划和异步执行任务。
- **创建并使用自定义代理**实现无障碍功能，添加高对比度和浅色模式切换开关。
- **使用 Copilot agents 页面作为控制中心**，监控并引导无障碍功能会话。
- **审查并迭代 cloud agent 的拉取请求**，通过提及 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 环境的练习。如果想继续探索，其他环境可以补充你在此处实践的内容：

- 🖥️ **[VS Code 环境](../../vscode/)**：直接在 IDE 中探索 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)**：从终端交付两个经过审查的更改，使用 Plan 和 Autopilot 模式、代理技能、QA 自定义代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率 issue 分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub issue 分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
