---
title: "第 2 课 - 添加星级评分：快速入门"
description: "使用 Copilot CLI 小幅修改游戏卡片，在转发的浏览器中审查更改，并通过合并首个拉取请求交付成果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以首次修改项目了。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示。你将让智能体展示评分、审查更改，并通过合并首个拉取请求完成交付。

在本课中，你将：

- 在功能分支上启动专注的 Copilot 对话。
- 请求智能体小幅修改项目。
- 使用 `/diff` 审查更改。
- 运行应用，在转发的浏览器中确认更改。
- 创建并合并首个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，该评分已经显示在游戏详情页上。不过，主页上的游戏卡片目前只显示标题、类别、发行商和简介。作为热身练习，你将让智能体在每张卡片上显示现有评分。这是适合首次会话的小型独立更改。

## 对话的组成

**对话**是你使用 Copilot CLI 处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的仓库和 Git 分支，而不会创建专用工作树。保存的对话可以让你稍后回到同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你的提示和智能体回复、智能体探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

先启动新对话，以开始实现功能。

1. 返回 Codespace。
2. 如果终端未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，运行：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，查找显示游戏详情的文件，并创建所需代码。现在已使用 Copilot CLI 添加一项新功能。

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使更改很小。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff`，检查每个已更改文件。
2. 确认游戏卡片在有评分时显示数字评分，`starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试涵盖这两种状态。
4. 审查 Copilot 运行的检查结果，并要求它修复任何失败。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此具体代码可能不同。请审查行为，不要要求实现与某个示例完全一致。

## 检查更改

不要只读代码就假设功能正常。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面（UI）。

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示满分为五分的评分。
4. 返回 Copilot，并让它停止刚才启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并首个拉取请求

功能已经完成，现在创建拉取请求，将新代码合并到项目中。

1. 请求默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。创建拉取请求时，Copilot CLI 会推送现有提交并显示拉取请求 URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择 Copilot CLI 显示的 URL，以打开拉取请求。
4. 审查已更改文件和检查结果。
5. 确认可以合并后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

你已使用 GitHub Copilot CLI 交付首个更改。具体来说，你：

- 在功能分支上启动了专注的 Copilot 对话。
- 指示智能体修改游戏卡片。
- 使用 `/diff` 审查了更改。
- 运行应用，在转发的浏览器中确认了星级评分。
- 创建并合并了首个拉取请求。

接下来从筛选议题开始，使用 Plan 和 Autopilot 模式[构建更大的功能][next-lesson]。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
