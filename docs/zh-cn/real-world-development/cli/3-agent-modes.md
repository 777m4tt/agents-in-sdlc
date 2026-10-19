---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

刚才我们为项目添加了一项小功能。较大的更改则需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作方式，帮助团队正确地构建所需内容。这是系列课程的第一课：你将从议题出发，按常见的智能体开发流程生成新功能、验证代码、确认行为，最后成功合并。

> [!NOTE]
> 后续功能开发将继续使用同一对话和分支。通常，不同文件类型的工作会放在不同分支或拉取请求（PR）中；本研讨会为了聚焦核心概念而简化了流程。

在本课中，你将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式下定义需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现该功能时，你还将更新仓库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩展，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了功能，但在编码前需要确定一些细节，例如如何组合类别。你将使用 Plan 模式明确这些决定，然后授权 Autopilot 在限定范围内实现。

## 背景

在开发流程中引入 AI 编码智能体，并不会改变软件开发的基本原则。多数开发者遵循类似流程：

1. 从描述需求的议题开始。
2. 规划要构建的内容。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但通常与以上步骤相近。

遵循标准流程，可以确保 AI 生成的代码符合既定要求，并像人工编写的代码一样经过审核。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并反复迭代，直到任务完成。

先在 Plan 模式下审查计划，然后使用 Autopilot 实施。

## 从议题开始

开始筛选功能前，返回 Codespace，确保仓库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后创建筛选功能分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

现在可以看到提示以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划可以在开始实现功能或执行任务前明确方案。处理复杂任务时，花时间制定计划通常很有帮助。切换到 Plan 模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用之后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定合适方案，以制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答；没有唯一的正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看计划。
6. 如果计划中的任何部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划完成并审查后，就可以开始实现。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，Copilot 可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，与其他代码一样，合并前需要审查。检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与议题及规划时做出的决定进行比较。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并要求 Copilot 解决任何失败。

## 体验新功能

代码看起来没问题，但是否能正常运行？像之前一样启动应用，通过 Codespaces 转发的端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、发行商筛选，以及按照计划确定的筛选组合。
4. 确认重置和无结果时的行为符合议题和规划决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审查了功能。在本课中，你：

- 从 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式下定义了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来通过[自定义指令][next-lesson]深入了解代码生成，并确保代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
