---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能（AI）助手。你可以在仓库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业经验编码为可复用智能体，例如确保符合 [Web 内容无障碍指南（WCAG）][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局 `~/.copilot/agents` 中定义。每个文件包含至少有 `name` 和 `description` 的 YAML（YAML Ain't Markup Language）frontmatter，后面是定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有一些重叠：两者通常都通过 Markdown 文件定义，并指导 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行工作的角色，技能是工具**。

自定义智能体有自己的上下文窗口，负责协调技能（甚至其他智能体）完成工作。本实验中，无障碍自定义智能体会根据无障碍指南审查和更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一的正确编写方式。与 AI 的其他用法一样，应通过测试和迭代找到适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问众筹平台，无论其视觉能力或偏好如何。近期用户反馈指出，有些用户觉得当前深色主题难以阅读，因为文本与背景的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍至关重要，因此你要使用自定义智能体尽快生成此功能。

在本练习中，你将：

- 检查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 检查无障碍自定义智能体

项目已为无障碍工作创建了自定义智能体。先检查其内容，了解它如何引导 Copilot。

返回 codespace，然后打开终端，从 `main` 创建用于无障碍工作的独立分支（将练习 3 的筛选 PR 与本次工作分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分并检查其中的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 代码示例。
   - 常见问题和错误清单。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的自定义智能体。选择自定义智能体后，该聊天会话将使用相应的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选中 **Agent**，请先选择它。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（默认显示 **default**）。
3. 从可用智能体中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会以内联方式显示在聊天中，包括文件路径和差异，供你审查。
6. **Files changed** 指示器会随着智能体修改文件而更新。

> [!NOTE]
> 这可能需要几分钟。Copilot 会实际修改仓库，例如更新 Astro 组件、CSS 和相关测试。

下一课中，你将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。借助自定义智能体，可以将团队专业知识和标准编码为可复用角色，引导 Copilot 更有效地完成特定工作。

你学习了：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 代理模式中使用自定义智能体。

接下来[监控并引导智能体的工作][next-lesson]，在更改生成过程中进行审查，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
