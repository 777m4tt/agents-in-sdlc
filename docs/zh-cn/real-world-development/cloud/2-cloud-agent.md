---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务，例如未解决的安全问题、需要更新的旧代码，或因时间不足而长期搁置的功能请求。GitHub Copilot cloud agent 能自主执行更新代码和添加功能等任务。完成后，它会创建草稿拉取请求（PR），供开发者审查。这样可以委派繁琐任务、加快开发进程，并让开发者专注于更重要的工作。

你将了解如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初创建网站的承包商没有留下完善的文档，实际上完全缺失文档。团队希望先为应用中所有导出函数添加 TypeScript 文档（TSDoc）注释。

此外，设计团队希望改进游戏发现体验：在每款游戏的详情页显示同类别的其他游戏，方便访客继续浏览。他们暂时不需要完善的实现，只需要可用于用户体验（UX）验收测试的内容。目前这是一个阻塞项，但优先级低于其他议题。

这类任务很容易被推迟，适合分配给 Copilot cloud agent。它可以异步处理任务，让开发者先做其他工作，之后再回来审查结果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台执行任务。你可以通过多种方式分配任务，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求跟踪进度、设置环境并开始工作。你可以在会话进行中或完成后查看。解决方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 编写范围明确的指令

GitHub Copilot 并非魔法，不会让你免于思考。看似简单的操作也可能包含复杂性，因此要谨慎[确定 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、尝试并逐步调整。引入生成式 AI 不会改变软件开发的基本原则。

## 本仓库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果你已完成自定义指令练习，就已经见过 `.github/copilot-instructions.md` 的作用；否则，现在可以快速查看。将工作分配给 Copilot cloud agent 前，先只读检查仓库现有指令文件，以便稍后观察其影响。

在仓库的 GitHub Web 界面或正在运行的 codespace 中打开：

- `.github/copilot-instructions.md`：查看 **Code standards**，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看将指令范围限定到 `**/*.test.ts` 文件的 `applyTo` frontmatter。

下一节将 `Code lacks documentation` 议题分配给 cloud agent 时，留意生成的 PR 是否包含 TSDoc 注释、遵循 TypeScript 约定并添加文件注释头。稍后的 PR 审查练习还会再次关注这些内容。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境并运行设置脚本，确保环境正常。将任务分配给 Copilot 时也是如此；它的工作方式类似软件工程师（SWE）。

Cloud agent 使用 [GitHub Actions][github-actions] 环境。你可以通过创建[专用设置工作流][setup-workflow]来定制环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml`，并会在 Copilot 开始工作前运行。它可以提供开发工具和依赖项。本实验已预先配置工作流，以便顺利完成练习。它会提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用程序所需的 SQLite 数据库迁移和种子数据：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流相似，但有两个要点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行该作业。
- `workflow_dispatch` 触发器允许你从仓库的 **Actions** 选项卡手动运行工作流。这样可以先测试工作流是否成功，无需等待 Copilot 触发。

## 添加文档

很多项目的文档已经过时或完全缺失，这会降低效率，增加维护代码库或帮助新开发者加入团队的难度。这类技术债务常常长期无人处理。Copilot 擅长生成文档，因此很适合将此任务分配给 Copilot cloud agent。它会在后台生成所需文档，后续练习中你将审查结果。

1. 在新浏览器选项卡中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，其标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现 PR，请等待片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项清单。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办清单，因为它正在创建环境（运行前面介绍的工作流）、分析项目并确定处理任务的方法。

13. 审查清单和 Copilot 将要完成的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到状态更新。

16. 查看实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并确定方案，以及编写代码。

这可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让开发者可以处理其他工作。接下来利用这一点，再分配一个任务给 Copilot cloud agent，然后开始编写应用程序功能。

## 为游戏详情页添加相关游戏区域

Copilot cloud agent 的一项优势是可以分配工作，让你专注于一组任务，而它处理另一组任务。即使为设计团队添加相关游戏区域并不耗时，这段时间也可以用来处理其他工作。现在将此任务分配给 Copilot cloud agent。

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

随后，议题的首条评论中应出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式类似软件工程师，无需持续监控；等任务完成后审查结果即可。接下来开始创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将议题分配给 cloud agent，让它异步处理技术债务、实现功能，或协助迁移到其他框架。

你学习了：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

cloud agent 正在后台工作，现在可以开始创建并使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并能读取前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
