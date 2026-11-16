---
title: "第 1 课 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI，熟悉项目并找到预置的筛选 issue。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是一款功能强大的代理式编码助手，可在终端运行，让你通过命令行探索代码库、生成代码、运行命令并与外部工具交互。它可以帮你委派任务、请求更改并保持专注。第一步当然是安装工具。好在可以使用熟悉的工具完成安装。

在本课中，你将：

- 使用 npm 安装 GitHub Copilot CLI。
- 使用 GitHub 账户完成身份验证。
- 信任工作坊存储库并尝试快速对话。
- 通过内置的 GitHub 模型上下文协议（MCP）服务器查找筛选 issue。

## 场景

团队正开始使用人工智能（AI）代理处理不断增长的待办事项。Copilot CLI 将这种能力带入许多开发者日常使用的终端。本课将完成安装和身份验证，为整个工作坊的后续学习做好准备。

## 安装 Copilot CLI

可以通过 [npm][install-cli]、WinGet 或 Homebrew 安装 Copilot CLI。由于 GitHub Codespaces 已预装 Node.js，本练习使用 npm。

1. 返回 Codespace 并打开终端。
2. 验证 Node.js 已安装且满足版本要求：

   ```bash
   node --version
   ```

   应看到版本号为 24 或更高。

3. 全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

4. 验证安装：

   ```bash
   copilot --version
   ```

   应能看到显示的版本号。

## 使用 GitHub 完成身份验证

首次启动 Copilot CLI 时，系统会提示你使用 GitHub 账户完成身份验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果出现提示，请按设备代码说明进行身份验证并授权 Copilot CLI。
3. Copilot CLI 会显示以下提示：

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 确认显示的路径是 Tailspin Toys 存储库，然后选择 **Yes, and remember this folder for future sessions** 表示信任。

> [!NOTE]
> 在 Codespace 中，你可能已经通过 GitHub 会话完成身份验证。如果 Copilot CLI 启动时没有要求身份验证，就可以继续使用。

## 熟悉 Copilot CLI

普通 shell 提示符中的命令会直接在 Codespace 中运行。启动 Copilot CLI 后，自然语言会发送给代理，斜杠命令则用于控制对话。

1. 输入 `/model`，使用方向键选择 **Auto**，按 <kbd>Enter</kbd>，然后再次按 <kbd>Enter</kbd> 确认。
2. 输入 `/help` 查看你所安装版本中可用的命令，然后按 <kbd>Esc</kbd> 关闭帮助界面。
3. 向 Copilot 提出一个简单问题，确认一切运行正常：

   ```plaintext
   What are the key files in this project?
   ```

4. 阅读回复，留意 Copilot 如何先探索存储库再作答。
5. 输入 `/mcp list`，确认内置的 GitHub MCP 服务器可用。
6. 请 Copilot 查找筛选 issue：

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 打开 URL 并阅读 issue。完成第一个小更改后会用到它。

> [!TIP]
> 普通 Copilot CLI 会话会使用终端当前检出的分支，不会自动创建工作树。每次更改前都将创建功能分支。

## 使用工作坊快捷方式

Copilot CLI 通常会在使用已获许可的工具范围以外的工具前征求同意。本工作坊中，你将使用 `--yolo` 重新启动它。该快捷方式会在 Codespace 中移除这些审批提示，让你专注于练习。

> [!CAUTION]
> `--yolo` 会启用完整的自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。只能在 Codespace 或虚拟机（VM）等隔离环境中使用，绝不要将它设为日常开发的默认别名。详情请参阅[允许和拒绝工具使用][allow-all-warning]。

在本工作坊中，`--enable-all-github-mcp-tools` 会启用后续课程用于处理 issue 和拉取请求的 GitHub MCP 读写工具。Codespace 限制了对本地计算机的访问，但经过身份验证的 GitHub 资源仍是真实资源。发布或合并前，请审查更改。

1. 输入 `/exit` 退出 Copilot CLI。
2. 在存储库根目录重新启动它：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再询问 Copilot 一个有关项目的简单问题，以确认对话正常工作，然后输入 `/exit` 退出。

Copilot 会自动保存对话。之后更新说明或添加代理时，可以使用 `copilot --resume` 返回同一功能对话和分支。

## 总结和后续步骤

恭喜！在本课中，你：

- 使用 npm 安装了 GitHub Copilot CLI。
- 使用 GitHub 账户完成了身份验证。
- 信任了工作坊存储库并尝试快速对话。
- 通过内置的 GitHub MCP 服务器找到了筛选 issue。

Copilot CLI 已安装完成。接下来开始[第一个专注的更改][next-lesson]，使用 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-cli]
- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
