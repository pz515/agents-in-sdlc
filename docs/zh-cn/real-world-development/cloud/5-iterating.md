---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 一起完成了多项任务，以改进用户体验并添加功能：请 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的无障碍功能。现在查看代码更改，必要时向 Copilot 提供反馈以改进成果。

### 场景

正如前面多次强调的，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。现在，在为团队其他成员启用审查前，先审查 GitHub Copilot 在文档、相关游戏功能和无障碍功能方面的建议。

## 安全和 GitHub Copilot 云智能体

由于 Copilot 云智能体会异步且无人监督地执行任务，因此设置了一些安全限制：

- Copilot 对存储库只有读取权限，对它用于编写代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），它为代码添加了文档。使用 GitHub.com 的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 对外部资源访问受限，包括调用外部 MCP 服务器。你可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 查看拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成。这可能需要几分钟，可以休息一下或回顾目前学到的内容。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改会有所不同。

   浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些来自练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有待审批的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 的检查队列。正常情况下，单体 Astro 应用的项目检查会全部通过。完成可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法提供固定的请求文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论请求 Copilot 修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上述建议，或提出其他希望在代码库中看到的文档更新。
2. 选择 **View Session** 观察 Copilot 工作。注意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 可返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。满意后，将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准。
7. 如果有待审批的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会进入 PR 的检查队列。正常情况下，单体 Astro 应用的项目检查会全部通过。完成可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到该分支，手动测试相关游戏功能。转到 Codespace 并打开终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。与之前一样，可以通过云智能体评论请求迭代更新，例如调整显示相关游戏的数量，或要求添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义说明）。在 **Conversation** 选项卡中发表评论即可，Copilot 会看到并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现时，特别留意：
   - 用于切换模式的 UI 组件；
   - 使用 local storage 保存用户偏好；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准。
8. 如果有待审批的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会进入 PR 的检查队列。正常情况下，单体 Astro 应用的项目检查会全部通过。完成可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能符合预期，并且页面重新加载后设置仍然有效。

留意无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 请求更新。

## 可选练习——继续委派任务

云智能体适合处理真实待办事项，让你能转而处理其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，逐步养成这种工作习惯。以下是一些想法：

- 在游戏详情页创建支持者意向表单；
- 为游戏列表页实现分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。本系列课程中，你：

- **检查了存储库中的自定义说明文件**，以便稍后观察它们对云智能体输出的影响；
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行；
- **创建并使用了无障碍自定义智能体**，添加高对比度和浅色模式切换开关；
- **使用 Copilot 智能体页面作为 mission control**，在无障碍会话进行时监控并引导智能体；
- **审查并迭代了云智能体创建的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体学习路径。若想继续探索，其他学习路径可以补充本次实践：

- 🖥️ **[VS Code 学习路径](../../vscode/)**：直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)**：在终端完成两个经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
