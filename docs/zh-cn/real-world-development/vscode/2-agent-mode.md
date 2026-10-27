---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。作为开发者，通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件，再进行修改……要做的事情还很多。

Copilot 代理模式正是为此而设计的。

Copilot 代理模式可以在 IDE 中更自主地工作。它的工作方式类似开发者：先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。现在了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 代理模式和云智能体的名称相似，但它们适用于不同的体验。代理模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队中的结对程序员，异步工作，并通过议题和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot 代理模式探索项目、识别相关文件并协调更改；
- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 在将代码合并到代码库前，审查 Copilot 代理模式生成的更改和测试。

## 场景

游戏列表不断增长，因此希望允许用户按类别和发行商筛选。上一练习已添加 publishers helper，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改任何内容之前，先探索 Tailspin Toys 网站，了解当前功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看游戏详情。它是一个单体 Astro 应用程序，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 front matter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能则通过小型、限定范围的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。可以按照以下步骤在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一项练习结束后，当前应位于 `main`。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到筛选工作分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的横幅，其中包含 URL：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，请关闭对话框（选择 **x**）。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端选择 localhost 链接会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 codespace 的私有隧道，Web 服务器运行在 codespace 中。

### 探索网站

网站启动后，可以探索它的功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后，会打开详情页，显示标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。模式和模型选择因练习而异；每一步会在相关位置说明。

网站的初始实现已经可用，现在要添加新功能来改进它。先查看待办事项。通过模板设置项目时，系统会自动创建一组议题；请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用了 Claude Sonnet 4.5，因此在说明中列出了该模型，希望由此获得相对一致的代码建议和良好体验。不过，由于 LLM 具有概率性，你可能会看到与说明不同的建议。这完全正常。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问存储库中的待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还根据其判断的用户价值为议题排了优先级。

## 查看说明文件

让智能体生成代码之前，先查看将用于向 Copilot 提供上下文的说明文件。接下来会使用[用户界面 (UI) 说明][ui-instructions]，了解如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中关于如何添加功能的整体指导，包括：
   - 架构概览；
   - 组件设计、可测试性和无障碍原则；
   - 指向不同文件类型具体说明的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目中的文件。路径相对于说明文件所在位置。这样可以复用说明、将复杂内容拆分为更小的部分，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用程序进行三项独立更新：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页并添加筛选 UI。

此外，合并前必须运行并通过测试。Copilot 代理模式可以执行这些任务，现在就来添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件。你会看到它找到数据层 helper、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会有 **Keep** 或 **Undo** 选项；工作尚未完成时无需选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中请求确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会有暂停，甚至出现测试失败。这没有关系。Copilot 会在生成代码和运行测试之间迭代，直到任务完成且未发现错误。

   ![Copilot 代理模式完整的 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你了解了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你学会了：

- 使用 GitHub Copilot 代理模式跨 UI 和数据层实现新功能；
- 让 Copilot 代理模式探索项目、识别相关文件并协调更改；
- 将代码合并到代码库前，审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，然后为其创建拉取请求。

### 扩展探索练习——实现分页

随着游戏列表不断增长，需要添加分页。运用本练习学到的技能，请 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件；
- 考虑分页方式：允许用户选择每页大小，还是使用固定值；
- 提供实现所需的指导，确保 Copilot 理解预期行为；
- 可能需要与 GitHub Copilot 多轮迭代，提供更改请求和上下文。这是使用 Copilot 时的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
