---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要准备好环境。你将创建自己的 Tailspin Toys 仓库副本，启动一个用于练习的 [codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已正常运行。

## 设置实验仓库

要创建代码仓库副本，请从[模板仓库][template-repository]创建一个实例。新实例包含实验所需的所有文件，后续练习都将在此副本中进行。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验仓库页面选择 **Use this template**，然后选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在拥有 GitHub Copilot 访问权限的组织中创建仓库。

   ![填写仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新仓库路径（**organization-or-user-name/repository-name**），稍后会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建仓库时，系统会自动为你创建一组 GitHub 议题。整个研讨会中都会使用这些议题，无需自行创建。

## 创建 codespace

接下来使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的集成开发环境（IDE），支持多种编程语言、扩展和工具。

1. 打开刚创建的仓库。
2. 选择绿色的 **Code** 按钮。

   ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

   ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但仍比手动安装所有服务快得多。等待时可以探索 GitHub Copilot 的其他功能，接下来会继续使用这些功能。

> [!CAUTION]
> 后续练习还会用到此 codespace。现在先让它保持打开，并留在浏览器选项卡中。

> [!NOTE]
> 本研讨会可在 codespace 或本地 [dev container][dev-containers] 中运行。两者都包含顺利完成练习所需的先决条件。如需在本地运行，请在 VS Code 中打开克隆的仓库；出现提示时选择 **Reopen in Container**，VS Code 会构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和代理模式

要使用 GitHub Copilot Chat 代理模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。使用 GitHub Codespace 时通常已预装此扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，可能需要使用 GitHub 账户进行身份验证。

1. 返回 codespace。
2. 如果编辑器右侧没有 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息，然后按 Enter。这会启动 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示登录 GitHub 账户。按提示完成身份验证。

   ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，应能看到 Copilot Chat 窗口。

## 总结

你已创建实验仓库副本，并开始创建 codespace。接下来将在其中编写代码。

## 下一步

现在开始使用 Copilot。继续阅读[练习 1：自定义指令][next-lesson]，向 Copilot 介绍项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
