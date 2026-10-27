---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求之前，应先在浏览器中确认功能确实正常。无需自己逐一操作应用程序，可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将：

- 了解 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 请 Copilot 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当代理模式能够访问编辑器之外的资源时，功能会强大得多。Model Context Protocol (MCP) 为 Copilot 提供了这种能力，它是一种让智能体与外部工具和服务通信的标准方式。

![代理模式与上下文、LLM 和工具交互的示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具、服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以使用这些 API 创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力。AI 智能体可以使用它浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器，可以访问不同的工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 从安全角度看，应将 MCP 服务器视为项目中的其他依赖项。使用前请仔细审查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该能看到已经配置好的 `github` 服务器：

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

项目模板中包含此 `github` 条目，因此 Copilot 能在上一练习中读取议题列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装任何内容。VS Code 通过 HTTP 连接服务器；Copilot 首次使用其工具时，需要使用 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了可操控的浏览器，正好用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列。文件应如下所示：

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

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在没有可见窗口的情况下运行浏览器，这对没有桌面环境的 codespace 来说是必需的。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装了 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过这一步，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了可操控的真实浏览器。无需亲自操作应用程序检查工作，智能体可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果。这是在不离开对话的情况下，快速确认功能是否符合预期的方式。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体可以像辅助技术一样，根据带有标签的结构化元素（按钮、链接、列表项）进行推理。这样，快速的功能检查也能兼顾基本的无障碍验证。

连接服务器并启动应用后，请 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行每项操作并报告结果。根据议题中的验收标准审查其总结。如果发现异常，请提出后续问题，或让它修复代码后再创建拉取请求。

> [!NOTE]
> 测试时，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队审查。首先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示的更改](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求的方式有很多，包括使用 github.com 和 GitHub 命令行界面 (CLI)。既然已经在使用 GitHub Copilot，就让它来创建 PR。它可以找到相关议题并创建与议题关联的 PR。

1. 转到 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示，请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会要求你登录 GitHub；按照提示授权即可。
5. 观察 Copilot 搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 使用该服务器在浏览器中手动测试筛选功能；
- 发布分支并为筛选工作创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，以便更有效地完成无障碍审查等专门任务。

## 资源

- [MCP 到底是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
