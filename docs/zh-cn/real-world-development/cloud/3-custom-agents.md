---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。你可以通过存储库 `.github/agents` 文件夹中的 Markdown 文件定义代理，为其提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业知识整理成可复用的代理，例如确保符合《Web 内容无障碍指南》（WCAG）要求的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含 YAML 前置内容，至少要有 `name` 和 `description`，后面是用于定义代理行为、专业知识和指令的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何完成工作。最简单的区分方式是：**自定义代理是执行工作的人员，技能是工具**。

自定义代理拥有独立的上下文窗口，并负责协调技能（甚至其他代理）完成工作。在本练习中，无障碍自定义代理会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一正确的方法。和其他 AI 用法一样，应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

你将探索以下内容：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期反馈指出，部分用户难以阅读当前暗色主题，因为文字和背景的对比度不足。为解决无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此要尽快实现此功能。你将使用自定义代理生成实现。

## 检查无障碍自定义代理

项目已经为无障碍任务创建了自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 Codespace，然后检查无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML 前置内容。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` 前置内容。

3. 查看后续部分，了解其中介绍的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误清单。

## 创建并分配 issue

Mission control 是管理环境中所有代理的中心位置。你可以在此处将任务分配给 Copilot cloud agent、监控任务，并重新引导或提供其他说明。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新 issue 对话框。
4. 选择 **Blank issue** 创建空白 issue。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建 issue。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

   ![Copilot cloud agent 分配界面，突出显示自定义代理和 Accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。借助自定义代理，可以将团队专业知识和标准整理成可复用代理，更有效地引导 Copilot 完成工作。

你探索了以下内容：

- 如何定义自定义代理。
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式。现在可以通过 mission control [监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为组织使用自定义代理做准备][org-custom-agents]
- [为企业使用自定义代理做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
