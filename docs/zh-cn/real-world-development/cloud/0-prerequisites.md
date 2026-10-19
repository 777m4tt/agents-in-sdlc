---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot cloud agent 练习前，需要准备好环境。你将创建自己的 Tailspin Toys 仓库副本，并启动一个 [codespace][codespaces]，用于编辑指令文件和审查 cloud agent 生成的工作。

## 设置实验仓库

要创建代码仓库副本，请从[模板仓库][template-repository]创建一个实例。新实例包含实验所需的所有文件，后续练习都将在此副本中进行。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验仓库页面选择 **Use this template**，然后选择 **Create a new repository**。

   ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果你参加由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可以在拥有 GitHub Copilot 访问权限的组织中创建仓库。

   ![填写仓库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新仓库路径（**organization-or-user-name/repository-name**），稍后会用到。

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

## 总结

你已创建实验仓库副本，并开始创建 codespace。后续练习中，你将在其中与 Copilot cloud agent 协作。

## 下一步

接下来为 cloud agent 添加自定义指令。继续阅读[练习 1：自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建仓库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
