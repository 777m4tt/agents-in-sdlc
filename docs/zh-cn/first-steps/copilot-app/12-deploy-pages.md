---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流，在合并前审查拉取请求，并验证 Space Quiz 已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开访问地址。Copilot 可以准备工作流并引导部署，但仍需审查更改并确认网站已上线。

在本课中，你将：

- 请求 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟踪部署进度并验证已发布的网站。

## 场景

Space Quiz 已经位于 GitHub 仓库中，但其他人还需要一种简单的方式来体验它。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖即可通过公开网址（统一资源定位符，URL）访问。

## 请求 Copilot 部署网站

在 `space-quiz` 的 Copilot app 会话中发送以下提示：

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![三阶段示意图：已合并的 space-quiz 拉取请求；GitHub Actions 中成功运行的 Deploy website 工作流，包含 Build website、Upload artifact 和 Deploy to Pages 步骤；以及位于 yourname.github.io/space-quiz/ 的在线 Space Quiz。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示要求智能体创建并合并拉取请求，以及更改仓库的 Pages 设置。继续操作前，先审查差异、部署权限和 GitHub 的审批提示。

## 审查工作流并验证部署

按以下步骤确认 README、工作流和线上网站均符合预期：

1. 确认仅当仓库中尚无 README 时才添加 README。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤。
3. 仅在所需的拉取请求检查通过后合并。
4. 跟踪 GitHub Actions 部署运行，直到成功。
5. 打开报告中的 URL，确认 Space Quiz 已加载。

如果智能体无法启用 Pages 或合并拉取请求，请按它提供的具体说明操作。只有部署成功且网站可以正常加载，才算完成任务。

## 总结和后续步骤

你已完成本课的目标：

- 请求 Copilot 配置了 GitHub Pages 部署。
- 在合并前审查了工作流和拉取请求。
- 跟踪部署并验证了已发布的网站。

Space Quiz 现已作为网站上线；你也亲自验证了部署，而不是只依赖智能体的报告。接下来继续学习[第 13 课：审查与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
