---
title: "练习 3 - 使用 Playwright 模型上下文协议（MCP）服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用 Agent Mode 构建了筛选功能。创建拉取请求（PR）前，应在浏览器中确认功能确实正常。与其手动浏览应用，不如连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能；之后再发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 提供新工具。
- 在工作区中添加 Playwright MCP 服务器。
- 请求 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议（MCP）？

当 Agent Mode 能够访问编辑器以外的资源时，它会更加强大。模型上下文协议（MCP）为 Copilot 提供了这种能力，是智能体与外部工具和服务通信的标准方式。

![Agent Mode 内部工作原理及其与上下文、大型语言模型（LLM）和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）交互的示意图](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，智能体可以实时访问最新信息（通过资源）并代表你执行操作（通过工具）。

智能体通过 MCP 服务器访问工具和资源。服务器负责管理智能体与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。不同的 MCP 服务器提供不同的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 仓库的 API，使智能体能够创建和更新仓库、管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，例如导航网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可提供不同工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，便于发现并为生态系统作出贡献。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样谨慎对待 MCP 服务器。使用前仔细审查源代码、验证发布者并考虑安全影响。只使用可信 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 应能看到已配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板已包含此 `github` 条目，因此上一练习中 Copilot 能读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用工具时需使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，可用于测试功能。

1. 在 `.vscode/mcp.json` 中添加与 `github` 并列的 `playwright` 条目，使文件如下所示：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 参数要求 Playwright 在没有可见窗口的情况下运行浏览器；codespace 没有桌面环境，因此必须使用此参数。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它，立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选操作。如果跳过，VS Code 会在下一步中 Copilot 首次需要工具时启动服务器并提示你信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后总结观察情况，无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器会根据页面的[无障碍树][playwright-mcp-server]工作，而不是截图。智能体据此处理结构化且带标签的元素（按钮、链接、列表项），与辅助技术类似，因此快速功能检查也能顺带验证一些无障碍问题。

连接服务器并启动应用后，请 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步执行操作并报告发现。根据议题验收标准审查报告；如果发现问题，可继续提问或要求修复代码，再创建 PR。

> [!NOTE]
> 测试需要应用运行在 `http://localhost:4321`。如果已停止开发服务器，请先重启。Copilot 首次使用 Playwright MCP 时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，创建拉取请求（PR），让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到仓库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建 PR。也可以让 Copilot 创建 PR，并关联对应议题。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉菜单中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示请求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时可能会提示进行 GitHub 身份验证，按提示操作即可。
5. 观察 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查 PR，但**不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 提供新工具。
- 在工作区中添加了 Playwright MCP 服务器。
- 使用它在浏览器中手动测试筛选功能，然后再发布。
- 发布了分支并为筛选功能创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
