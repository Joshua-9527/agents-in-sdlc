---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

前几项练习中，你让 Copilot cloud agent 执行了三个任务，以改进用户体验并添加功能。cloud agent 虽然可以异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制中心，可以查看所有带有未关闭拉取请求（PR）的代理任务、检查执行情况，也可以引导正在进行的会话。

在本课中，你将：

- 探索代理页面，监控 cloud agent 任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给代理后，团队意识到也可以同时添加浅色模式。网站样式和切换功能已经在修改中，因此一并实现浅色模式很合理。现在引导代理，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的应用程序编程接口（API）。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置模型上下文协议（MCP）服务器。
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 当前的工作状态和采用的方法而变化。

4. 同时留意右侧的拉取请求（PR）窗格。可以在这里查看 PR 和更改的文件，以便进一步监控。

## 引导 cloud agent

你已查看正在执行的任务。现在要求 Copilot 在实现高对比度模式时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题会根据 Copilot 使用的名称和当前工作状态而变化。

   ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示已完成设置并开始工作。出现类似下方的消息时，就表示已开始处理。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Copilot agents 页面中的 cloud agent 任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！完成当前步骤后，它会将新请求加入工作流程。

此过程可能需要几分钟。等待期间，可以暂停一下，回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心。你可以在此跟踪所有正在进行和已完成的任务、审查已执行的工作，也可以重新引导正在运行的任务以调整范围或补充说明。

你探索了以下内容：

- 使用 mission control 和代理页面监控 cloud agent 任务。
- 引导正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代审查 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
