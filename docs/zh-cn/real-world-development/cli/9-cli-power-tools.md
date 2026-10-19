---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话以及可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可用于访问高级功能、幕后信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他实用命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派到云智能体的可选命令。

## 场景

CLI 核心工作流已经完成。现在了解其他功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，你也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请在仓库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统指令、工具、消息和可用空间的上下文分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口逐渐填满时，Copilot CLI 会自动压缩上下文。需要自行决定压缩时机时，使用 `/compact`。如果要切换到无关任务并开始全新对话，则使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发者也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于改进 AI 工具的使用方式。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送给其他人或发布前，请审查导出的内容。会话导出可能包含提示、回复和项目详细信息。

发布链接或 gist 是可选操作。不要公开团队不打算分享的仓库或对话内容。

## 可选：扩展并行工作或委派任务

核心研讨会已完成。Copilot CLI 还提供了适用于较大型任务的命令：

- `/fleet` 可将独立子任务分配给多个子智能体并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot cloud agent，由其异步处理并可能创建拉取请求。

这些命令为可选项，因为它们可能会创建额外工作树或远程工作。尝试前先开始一个范围清晰的新任务，并按常规工作流审查结果。若想深入了解异步智能体工作，可继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

使用 GitHub Copilot CLI 的斜杠命令，可以配置工具、共享会话并了解幕后状态。在本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和委派到云智能体的可选命令。

Copilot CLI 还有更多斜杠命令值得探索。接下来[回顾所学内容][next-lesson]并了解后续学习方向。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
