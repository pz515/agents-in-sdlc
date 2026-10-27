---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题；
- 添加星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）；
- 从筛选议题开始，在 Plan 模式中确定方案、使用 Autopilot 模式实现，并在 Interactive 模式中审查；
- 使用自定义说明引导智能体，然后自定义现有 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查；
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中探索筛选功能；
- 创建并选择质量保证（QA）自定义智能体，评估需求、覆盖率、技能结果和浏览器证据；
- 审查完整的筛选更改，并为筛选 PR 授权 Agent Merge；
- 探索了用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 交付的内容

本研讨会有两个 PR 里程碑，每个都从更新后的 `main` 分支创建：

1. **星级评分**：在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流**：实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为了简化研讨会流程，这些工作合并到了一个 PR 中。

## 多种验证方式

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前汇总了这些结果，并审查需求和测试覆盖率。

新增测试应解决真实缺口；如果 QA 检查后不需要新增测试，也可能是正确结果。授权合并前，请审查代码和证据；更改后应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复执行的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前思考方案，使用 **Interactive** 跟进聚焦的更改，使用 **Autopilot** 处理范围明确的任务。常规编辑可选择更快的模型，复杂任务则选择能力更强的模型。

上下文和基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出。

## 继续探索

你已经完成核心工作流。还可以了解一些 CLI 功能：

- `/review`：请代码审查智能体分析更改。
- `/rubber-duck`：讨论问题并获得另一种观点。
- `/fleet`：并行协调独立子任务。
- `/worktree`：隔离独立任务。
- `/delegate`：将任务发送给 Copilot 云智能体。

## 后续步骤

要提升对任何工具的熟练度，最好的办法就是持续使用。可以将它用于生产代码、兴趣项目，或那个想了很多年却一直没开始的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
