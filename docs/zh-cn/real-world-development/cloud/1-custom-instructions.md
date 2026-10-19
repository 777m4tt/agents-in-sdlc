---
title: "练习 1 - 自定义指令（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能获取这些信息。[指令文件][instruction-files]可提供此类指导，让 Copilot 不仅了解要做什么，也了解如何完成。

在本练习中，你将：

- 探索项目上下文、编码指南和文档标准如何通过仓库自定义指令和路径范围指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的仓库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，此处不会运行修改前后的对照提示。Copilot cloud agent 在 GitHub 议题上异步工作，因此很难实时并排展示效果。稍后审查 cloud agent 创建的拉取请求时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 制定了开发实践指南和要求，包括：

- 数据层必须始终有单元测试。
- UI 应使用深色模式并呈现现代风格。
- 应通过 TypeScript 文档（TSDoc）注释为代码添加文档。
- 每个文件开头应有一段注释，说明文件用途。

你将使用指令文件向 Copilot 提供这些要求。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好设置，帮助它理解代码风格和要求。你可以指定编码约定、库和注释风格，也可以为整个仓库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：仓库每次请求都会发送的单个指令文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概览、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，这些指令文件仅用于 Copilot Chat 中的代码生成，不用于代码补全或下一步编辑建议。
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
> 每个使用 GitHub Copilot 的项目都应建立完善的指令文件集合。查看项目文件时，可以看到 UI 更新和 Astro 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。各个产品界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适用时说明。
>
> 如需模板或起点，可探索 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义智能体及其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

花些时间阅读仓库中的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，查看其中 Astro、Drizzle 数据层、测试等相关指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看 `applyTo` 字段；它设置相对于仓库根目录的 glob，用于确定指令适用的文件。这里，符合 `**/*.test.ts` 的 TypeScript 测试文件会匹配。
5. 查看项目中有关创建单元测试的指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意它指向其他指令文件（例如 `unit-tests.instructions.md`）和项目文件的链接。路径相对于指令文件本身，而非仓库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求添加代码文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的仓库标准

接下来需要进行的唯一编辑，是添加项目级规则，要求代码中包含 TSDoc 文档注释和文件级注释头。后续练习中，cloud agent 处理议题时会使用此规则。

编辑前先创建分支。cloud agent 会读取议题目标分支中的指令；此处创建分支是为了整洁地提交更改。

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

仓库级 `.github/copilot-instructions.md` 旨在向 Copilot 提供项目级信息。现在添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。它记录了项目编码标准，但尚未要求代码文档，这也是生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所述，指令可以放在仓库级 `.github/copilot-instructions.md` 中，供全局使用；也可以放在 `*.instructions.md` 中，针对特定语言、文件类型或任务。仓库级文件适合记录文档注释规则等项目标准。

## 提交、推送并合并指令更新

Cloud agent 会读取议题目标分支中的指令。下一练习中，将议题分配给 Copilot 时，它会从 `main` 创建分支，因此指令更改必须先合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。之后 cloud agent 处理下一练习中分配的议题时，就会读取这些指令。

> [!TIP]
> 如果你希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。这里使用分支是为了模拟真实项目的处理方式。

## 总结和后续步骤

你完成了本练习的目标：

- 探索了项目上下文、编码指南和文档标准如何通过仓库自定义指令和路径范围指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加了新的仓库级标准。

后续练习中，可以观察 cloud agent 创建的 PR 是否遵循此标准。

接下来将首个议题[分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
