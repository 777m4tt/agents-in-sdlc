---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义设置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace，安装 Copilot CLI，探索项目并找到已有的筛选议题。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并首个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建，并在 Interactive 模式下审查。
- 使用自定义指令引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议（MCP）服务器，并通过真实浏览器探索筛选功能。
- 创建并选择质量保证（QA）自定义智能体，以评估需求、覆盖率、技能结果和浏览器验证证据。
- 审查完整筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 里程碑，每个都从更新后的 `main` 创建独立分支：

1. **星级评分**：在游戏卡片上显示已有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并应用到功能、自定义 `quality-checks` 报告、创建 QA 配置，并包含相关测试。

从规划筛选功能到创建其 PR，你一直使用同一对话和分支。为简化研讨会，这些工作合并到同一 PR。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在创建 PR 前，将这些结果与需求审查和测试覆盖率检查汇总在一起。

新增测试应弥补实际缺口；如果 QA 运行后无需添加新测试，也可能是正确结果。合并授权前请审查代码和证据；更改之后应重新收集相关验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响其工作。本研讨会中，你更新了指令、自定义技能、创建 QA 配置并配置 MCP 服务器。可在不同对话间复用这些自定义设置，并根据团队需求调整。指令规定标准，技能描述可重复任务，自定义智能体定义专门角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不仅是智能体的摘要。

根据任务选择合适的**模式和模型**。先用 **Plan** 思考方案；小范围更改可用 **Interactive** 保持协作；范围明确的任务可用 **Autopilot**。常规修改可选择速度更快的模型，复杂任务则选择能力更强的模型。

上下文与基础设施同样重要。清楚说明要构建**什么**、**为什么**以及**如何构建**，会显著影响结果。

## 继续探索

你已完成核心工作流。还可以了解以下 CLI 功能：

- `/review`：请求代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获取其他视角。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

熟悉工具的最佳方式是继续使用它！将它用于生产代码、个人项目，或一直想构建的小应用。与团队分享经验，也向他们学习。还可以继续探索文档。

若想在终端中扩展 Tailspin Toys，可继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot cloud agent 研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
