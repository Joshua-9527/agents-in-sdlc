---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

使用 GitHub Copilot agent mode 时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作实时发生在你的计算机上，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以直接在聊天中审查差异、接受或拒绝单项更改，也可以通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

在本练习中，你将：

- 实时监控无障碍代理的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导代理，在高对比度功能旁添加浅色模式切换开关。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队又提出了新需求：同时添加浅色模式切换开关，供觉得默认主题太暗的用户使用。你将继续引导现有会话，扩展功能，而不是从头开始新对话。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中以内联方式显示差异，并附有 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 代理工作时，滚动聊天内容查看每次工具调用。你会看到文件读取、文件编辑以及代理请求运行的终端命令。
3. 代理建议编辑文件时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让代理再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看代理在本次会话中修改的完整文件列表。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式。完成后应会显示更改摘要。

## 引导代理添加浅色模式

高对比度模式已完成，现在在同一对话中添加浅色模式切换开关。由于继续使用同一聊天会话，代理仍保留此前工作的上下文，包括修改的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 代理会检查已有更改，并提出添加浅色模式的修改。
3. 像审查高对比度模式一样，逐项检查新编辑，并使用 **Keep** 或 **Undo** 控制每项差异。
4. 如果代理的方向不符合预期，例如试图以不希望的方式重构切换用户界面（UI），可选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清后的后续消息。

> [!TIP]
> 通过后续提示引导正在运行的会话，比从头开始更快。可以要求代理调整（“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式集成到现有设置面板”）或扩展范围（“同时为两个切换开关添加键盘快捷键”）。

## 审查更改集

提交前快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有已更改文件。你应能看到 Astro 组件、样式和相关测试的更新。
3. 打开一些已更改文件并查看差异。确认其中体现自定义代理的无障碍模式，包括无障碍富互联网应用（ARIA）属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat agent mode 监控并引导了正在进行的会话：检查逐项应用的文件编辑、接受或拒绝更改，并发送后续提示扩展工作，同时保留上下文。

在本课中，你：

- 实时监控代理的工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示引导正在进行的会话。

接下来，在[迭代 Copilot 的工作][next-lesson]中，将无障碍功能作为拉取请求发布，并审查本实验中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 agent mode][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
