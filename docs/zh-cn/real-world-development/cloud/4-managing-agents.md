---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三个不同任务，以改进用户体验并添加功能。云智能体可以自主异步工作，但监控这些任务仍然很重要。

有许多工具可用于管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，可以查看所有带有未关闭拉取请求（PR）的智能体任务，了解它执行的操作，甚至可以在会话进行时提供指导。

本课将：

- 使用智能体页面监控云智能体任务；
- 在会话进行时引导智能体添加其他功能。

## 场景

将创建高对比度模式的任务分配给智能体后，团队意识到也可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，就适合一并实现。你将引导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 转到 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码库文档；
   - 生成用于修改游戏的 API；
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码；
   - 创建 Copilot 工作环境；
   - 设置 MCP 服务器；
   - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 具体步骤会根据 Copilot 工作进度和采用的方法而有所不同。

4. 还要查看右侧显示的拉取请求面板，以便进一步监控 PR 和更改的文件。

## 引导云智能体

既然已经看到正在运行的任务，现在请 Copilot 在实现高对比度模式的同时添加浅色模式切换开关。

1. 选择用于添加高对比度模式的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。届时会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体任务页面中突出显示的引导活动会话对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，然后将新请求加入工作流程。

这同样需要几分钟，可以趁此暂停片刻，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过 mission control，可以跟踪所有活动中和已完成的任务、审查工作，并在任务进行中调整范围或补充指导。

本课介绍了：

- 使用 mission control 和智能体页面监控云智能体任务；
- 在会话进行中请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
