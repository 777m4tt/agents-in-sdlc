---
title: "练习 2 - 使用 Copilot Agent Mode 添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并运行测试。开发者通常需要查找相关文件、修改内容、运行测试、调试、找出遗漏文件并再次更新。这一过程可能不断重复。

Copilot Agent Mode 正是为此而设计。

Copilot Agent Mode 可以在 IDE 中更自主地工作。它会先探索项目结构，然后执行所需更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> Agent Mode 和 cloud agent 名称相似，但提供不同体验。Agent Mode 在 IDE 中执行任务，便于快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解：

- Copilot Agent Mode 如何探索项目、识别相关文件并协调多处更改。
- GitHub Copilot Agent Mode 如何跨用户界面（UI）和数据层实现新功能。
- 合并代码前如何审查 Copilot Agent Mode 生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望用户能够按类别和发行商筛选。上一练习中已经添加了发行商 helper；现在使用 Copilot Agent Mode 完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

进行更改前，先探索网站，了解其现有功能。

网站是一个带开发者主题的桌游众筹平台，可以列出游戏并显示游戏详情。它是单个 Astro 应用程序，在构建时将页面渲染为静态超文本标记语言（HTML）。页面通过 frontmatter 使用 Drizzle ORM 直接查询本地 SQLite 数据库；没有独立后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能使用少量作用域化 Astro `<script>` 和标准文档对象模型（DOM）API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭它。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的私有隧道，网站服务器正在其中运行。

### 探索网站

网站启动后，可以探索现有功能：

- **主页**：显示游戏列表，以及标题、图片和简介。
- **游戏详情页**：选择游戏后会打开详情页，其中包含游戏标题、简介、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

网站初始实现已可正常运行，但还可以增加功能。先查看待办事项。从模板设置项目时，系统已自动创建一组议题；请让 GitHub Copilot 显示这些内容。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。Agent 可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者不偏好某个模型。编写本研讨会时使用了 Claude Sonnet 4.5，因此将其写入说明，希望获得较一致的代码建议。由于大型语言模型具有概率性，实际建议可能不同，这是正常且可预期的。

> [!NOTE]
> Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 发送以下提示，询问 Copilot 仓库中的待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

Copilot 还会根据对用户的价值对事项进行优先级排序。

## 审查指令文件

让智能体生成代码前，先查看用于提供上下文的指令文件。本练习将使用[用户界面（UI）指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中说明了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看文件中的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型专用指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件位置，因此可以复用内容、拆分复杂指令，或提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三项更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 为筛选行为添加或更新测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并代码前必须运行测试并确认通过。Copilot Agent Mode 可以代为执行这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图智能体下拉菜单中仍选中 **Agent**。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并查找相关文件，包括数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库复杂度，这可能需要几分钟。过程中编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有相应按钮；无需在工作进行中选择它们。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中要求用户确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能会有测试暂时失败。这没有问题；Copilot 会在生成代码和测试之间迭代，直到任务完成且未检测到错误。

   ![Copilot Agent Mode 完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回仍在运行的网站，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习中，你使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加了新功能，并了解了：

- GitHub Copilot Agent Mode 如何跨 UI 和数据层实现新功能。
- Copilot Agent Mode 如何探索项目、识别相关文件并协调多处更改。
- 合并代码前如何审查 Copilot Agent Mode 生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并为此创建拉取请求。

### 加分探索练习 — 实现分页

游戏列表增长后，需要支持分页。运用本练习学到的技能，提示 Copilot 为网站实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 确定分页方式，例如由用户选择每页条目数，还是固定页大小。
- 提供 Copilot 所需的指导，使其按预期实现功能。
- 必要时迭代并提供上下文，请 Copilot 修改。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的 Agent Mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
