---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

整个实验中，你使用 GitHub Copilot 执行了多个改善用户体验和增加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在检查代码更改；必要时向 Copilot 提供反馈以改进结果。

### 场景

正如之前多次提到的，引入生成式 AI 并不会改变软件设计和开发运维（DevOps）的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在启用团队其他成员的审查之前，先检查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## 安全和 GitHub Copilot cloud agent

Copilot cloud agent 异步且自主地执行任务，因此设置了安全限制：

- Copilot 只能读取仓库，并且**只能**写入用于生成代码的分支。
- Cloud agent 在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 任何 GitHub Actions 工作流都必须经过人工批准才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，模型上下文协议（MCP）服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 生成的第一个拉取请求（PR）：为代码添加文档。通过 GitHub.com 的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**：Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。如有需要，可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests** 打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。请等待 Copilot 完成，这可能需要几分钟；也可以利用这段时间回顾已经学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 检查新添加的 TSDoc 文档注释和其他文档。具体更改可能不同。留意 TSDoc 注释、TypeScript 约定和文件注释头；这些要求来自练习 2 中检查的自定义指令文件。
6. 确认更新合适后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果工作流正在等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应进入 PR 检查队列。理想情况下，单个 Astro 应用程序的项目检查会全部通过。完成可能需要几分钟。

## 请求 GitHub Copilot 修改内容

你可以在 PR 评论或代码行内评论中像提及团队成员一样提及 Copilot。Copilot 会读取评论，并启动新会话来处理请求。由于结果并非确定，无法提供固定的请求文本。可以要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用程序的结构。

1. 添加评论，像提及用户一样标记 **@copilot**，请求修改生成的文档。可以使用以上任一建议，也可以提出其他文档改进要求。
2. 选择 **View Session** 观看 Copilot 工作。留意它如何启动新会话来更新内容。
3. 选择 **Back to pull request** 返回 PR。

   ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成后，PR 中应出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可继续迭代，直到满意为止。确认满意后，可以将 PR 从草稿转为准备审查状态，然后合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新合适后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果工作流正在等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流应进入 PR 检查队列。理想情况下，单个 Astro 应用程序的项目检查会全部通过，这可能需要几分钟。
9. **可选：**可在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端，运行以下命令，并将 `<branch-name>` 替换为 Copilot 创建的分支名称（例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。与之前一样，可以迭代并请求 Copilot 更新，例如调整显示的相关游戏数量，或补充文件注释头和 TSDoc 文档注释。请记住，分配此任务时尚未更新自定义指令。与之前一样，可以在 **Conversation** 选项卡中评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后审查通过无障碍自定义智能体实现的功能。此 PR 应包含练习 3 中要求的高对比度模式，以及练习 4 中在 Mission Control 请求的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现时，重点检查：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更新合适后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果工作流正在等待批准，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应进入 PR 检查队列。理想情况下，单个 Astro 应用程序的项目检查会全部通过，这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后设置仍然保留。

可以看到，无障碍自定义智能体引导 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或有改进建议，可以像之前一样在评论中标记 **@copilot** 请求更新。

## 可选练习 — 继续委派

Cloud agent 最适合处理真实待办事项，让你可以转而处理其他工作。可在仓库中创建更多议题并分配给 Copilot，尝试养成这种工作方式。例如：

- 在游戏详情页创建支持者兴趣表单。
- 在游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 工作流。在这些课程中，你：

- **检查了仓库中已有的自定义指令文件**，以便稍后观察 cloud agent 输出受其影响的方式。
- **将议题分配给 Copilot cloud agent**，观察它设置环境、规划并异步执行任务。
- **创建并使用了无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代了 cloud agent 的拉取请求**，通过标记 `@copilot` 请求修改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 工作流。若想继续探索，其他工作流可补充本次练习：

- 🖥️ **[VS Code 工作流](../../vscode/)** — 在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 工作流](../../cli/)** — 在终端交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的仓库中尝试：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
