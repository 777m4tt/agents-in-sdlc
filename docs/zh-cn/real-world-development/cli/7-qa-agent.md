---
title: "第 7 课 - 创建并使用质量保证（QA）智能体"
description: "创建 QA 自定义智能体，结合议题要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到自定义智能体中，定义清晰、一致的 QA 流程。

在本课中，你将：

- 探索自定义智能体如何配合指令、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

在创建拉取请求（PR）之前，Tailspin Toys 希望统一审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件描述智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

本课程中用到的自定义功能各有职责：仓库指令描述团队标准；`quality-checks` 技能封装可重复的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 使用这些能力评估需求并报告发现。它不会替代其他功能，也不需要新建对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。配置应说明 QA 要执行哪些检查，以及必须遵守哪些边界。

1. 返回 Codespace，确认筛选对话处于 Interactive 模式。
2. 请求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用智能体前，检查其配置，确认 Copilot 记录了预期的 QA 流程和边界，避免配置不完整或权限过宽。

1. 输入 `/diff`，打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 是可选字段，但添加后可提供清晰的显示名称。
3. 阅读配置说明，确认 QA 从需求开始、遵循仓库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供证据，修改实现代码前会征求同意，也不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏上述职责或边界，请在继续前要求默认智能体修订。

## 根据议题运行 QA

创建配置后，Copilot CLI 需在对话开始时加载项目智能体。恢复同一筛选对话并选择 QA，以便它使用已有的议题和计划决定。

1. 使用以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用列表中，但仍可通过上述命令启用。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体完成工作。
4. 阅读它提供的报告。

## 总结和后续步骤

你为工作流添加了可复用的专用角色，并审查了其结果。在本课中，你：

- 探索了自定义智能体如何配合指令、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查了其发现。

现在，实现、技能更新、QA 配置、测试和验证报告都已准备好供审查。接下来在功能 PR 中[汇总这些内容并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
