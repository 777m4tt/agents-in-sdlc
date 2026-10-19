---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能获取这些信息。[指令文件][instruction-files]是提供代码生成指导的重要方式。

在本练习中，你将：

- 探索项目上下文、编码指南和文档标准如何通过仓库自定义指令和路径范围指令文件传递给 Copilot。
- 在当前指令生效时，使用 Copilot 生成筛选功能的第一个数据层切片（发行商 helper）。
- 向 `.github/copilot-instructions.md` 添加新的仓库级标准。
- 重新运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合设定的标准，因为 Copilot 具有概率性。本练习的重点是观察更新指令后行为的整体变化，而非要求输出与示例逐字一致。

## 指令文件

### 场景

Tailspin Toys 制定了开发实践指南和要求，包括：

- 数据层必须始终有单元测试。
- 用户界面（UI）应使用深色模式并呈现现代风格。
- 应通过 TypeScript 文档（TSDoc）注释为代码添加文档。
- 每个文件开头应有一段注释，说明文件用途。

你将使用指令文件向 Copilot 提供这些要求。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好设置，帮助它理解代码风格和要求。你可以指定编码约定、库和注释风格，也可以为整个仓库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：仓库每次请求都会发送的单个指令文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概览、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境（IDE）中，这些指令文件仅用于 Copilot Chat 中的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用仓库级指令和包含 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。示例项目展示了一种典型方法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指令。
- 使用自然语言，保持指导清晰，并给出代码应遵循或避免的示例。

指令文件没有唯一的编写方式，AI 的使用方式也是如此。通过尝试找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应建立完善的指令文件集合。项目中包括 UI 更新和 Astro 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。各个产品界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适用时说明。
>
> 如需模板或起点，可探索 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义智能体及其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

阅读仓库中的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 指导。
3. 打开 `.github/instructions` 文件夹，查看其中 Astro、Drizzle 数据层、测试等相关指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。`applyTo` 字段设置相对于仓库根目录的 glob，确定指令适用的文件；例如符合 `**/*.test.ts` 的 TypeScript 测试文件会匹配。
5. 查看项目中有关创建单元测试的指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意它指向其他指令文件（如 `unit-tests.instructions.md`）和项目文件的链接。路径相对于指令文件本身，而非仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

先为本次更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

先使用当前指令发送提示，之后更新指令并重新发送相同提示，以观察差异。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应位于何处。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以允许 Copilot 修改文件。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会修改 helper 文件及其测试。
6. 留意建议的 helper 是一个带类型的函数，首个参数是 `db` 客户端，返回带类型的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 留意建议代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。这没有问题；本练习关注的是更新指令后行为是否更一致。

## 添加新的仓库标准

`.github/copilot-instructions.md` 为 Copilot 提供项目级信息。现在添加编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。它记录了项目编码标准，但尚未要求代码文档，这也是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 仓库级 `.github/copilot-instructions.md` 适合记录文档注释规则等全局项目标准。`*.instructions.md` 则适用于特定语言、文件类型或任务。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空缓冲区。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看新文件开头是否出现类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 查看函数是否包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已经引导 Copilot 遵循新的项目标准，并将其应用于下一练习将继续使用的真实代码。

## 提交指令并推送分支

指令文件与仓库中的其他文件一样，应通过源代码管理维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认指令更新和 helper 更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为 helper 基础实现修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求，并关联待办事项中的文档标准议题。可以让 Copilot 查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已在工作区中配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按提示授权；后续练习会介绍这种连接如何工作（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 请求 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新建的发行商 helper 基础实现：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并在本地切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求执行 GitHub 操作和 shell 命令时，检查命令并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 探索了项目上下文、编码指南和文档标准如何通过仓库自定义指令和路径范围指令文件传递给 Copilot。
- 在现有指令生效时，使用 Copilot 生成了发行商 helper 数据层切片。
- 向 `.github/copilot-instructions.md` 添加仓库级标准。
- 重新运行相同提示，观察建议的代码如何采用新标准。
- 将指令更新和 helper 基础实现提交到 `main`，供下一练习继续使用。

接下来使用[代理模式][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
