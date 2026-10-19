---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。你可以在仓库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业经验编码为可复用智能体，例如确保符合 [Web 内容无障碍指南（WCAG）][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局 `~/.copilot/agents` 中定义。每个文件的 YAML（YAML Ain't Markup Language，YAML 不是标记语言）frontmatter 至少包含 `name` 和 `description`，后面是定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有一些重叠：两者通常都通过 Markdown 文件定义，并指导 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是工具**。

自定义智能体有自己的上下文窗口，负责协调技能（甚至其他智能体）完成工作。本实验中，无障碍自定义智能体会根据无障碍指南审查和更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一的正确编写方式。与 AI 的其他用法一样，应通过测试和迭代找到适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

通过自定义智能体，你将了解：

- 如何定义自定义智能体。
- 如何向自定义智能体分配任务。

## 场景

Tailspin Toys 致力于让所有用户都能访问众筹平台，无论其视觉能力或偏好如何。近期用户反馈指出，有些用户觉得当前深色主题难以阅读，因为文本与背景的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍至关重要，因此你要使用自定义智能体尽快生成此功能。

## 检查无障碍自定义智能体

项目已为无障碍工作创建了自定义智能体。先检查其内容，了解它如何引导 Copilot。

返回 codespace，然后查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分并检查其中的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误清单。

## 创建并分配议题

Mission control 是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务、重定向会话并提供额外指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从列表中选择 **Accessibility agent**。

   ![云智能体分配界面截图，其中突出显示了自定义智能体和 accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。通过自定义智能体，可以将团队专业知识和标准编码为可复用角色，引导 Copilot 更有效地完成特定工作。

你学习了：

- 如何定义自定义智能体。
- 如何向自定义智能体分配任务。

Copilot 正在实现高对比度模式。接下来通过 Mission Control [监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
