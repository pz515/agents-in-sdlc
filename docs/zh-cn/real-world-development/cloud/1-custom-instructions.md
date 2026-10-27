---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 应该了解一些背景信息，就需要确保它能访问这些上下文。[说明文件][instruction-files]用于提供此类指导，帮助 Copilot 不仅理解要做*什么*，还理解应该*如何*完成。

本练习将：

- 了解项目专属上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot；
- 向 `.github/copilot-instructions.md` 添加一项新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，这里不会运行更新前后的提示进行对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示效果。稍后的练习中，你可以在审查云智能体创建的拉取请求时看到说明文件的影响。

## 说明文件

### 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式，并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件顶部都应添加一段注释，说明该文件的作用。

使用说明文件可以确保 Copilot 获得正确的信息，并遵循这些实践要求完成任务。

### 自定义说明

自定义说明可用于向 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。可以指定偏好的编码约定、库，甚至希望包含的注释类型。既可以为整个存储库创建说明，也可以为特定文件类型创建说明，以提供任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：这是一个针对该存储库的**每次**请求都会发送给 Copilot 的单一说明文件。它应包含项目级信息，也就是大多数 Copilot Chat 或 CLI 请求都会用到的上下文，例如技术栈、项目概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可以针对特定任务或文件类型创建这些文件。例如，为特定语言（如 TypeScript 或 Astro）提供指导，或为创建 UI 组件、新增单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 的代码生成，不会用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` front matter 的 `*.instructions.md` 文件。
>
> Copilot [还支持采用其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本研讨会的范围，不过示例项目展示了一种具有代表性的做法。总体而言：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码应该如何编写以及不应该如何编写的示例。

创建说明文件没有唯一正确的方法，就像使用 AI 也没有唯一正确的方法一样。通过不断尝试，可以找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该具备一套完善的说明文件。查看本项目中的文件时，可能会发现其中涵盖了许多任务类型，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面对这一功能的展示方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应界面的课程会在相关位置介绍。
>
> 想找模板或起点？可以看看 [awesome-copilot][awesome-copilot]，这是一个汇集说明文件、自定义智能体和其他资源的存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义说明文件

花些时间阅读此存储库随附的说明文件：其中包含核心文件 `copilot-instructions.md`，以及一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览该文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览，了解 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于决定说明适用于哪些文件。这里，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 留意其中为本项目编写单元测试的专门说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意其中指向其他说明文件（如 `unit-tests.instructions.md`）以及项目现有文件的链接。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 指出生成代码时可参考的示例。（这些路径是相对于说明文件本身，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但目前还没有要求在代码中编写文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

接下来要做的唯一编辑，是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。云智能体在本学习路径后续处理分配给它的议题时会读取这项规则。

编辑之前，先创建一个分支（云智能体会读取议题目标分支中的说明，但仍建议用分支整洁地提交更改）：

1. 在 codespace 终端中，创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库补充编码标准，以改善代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该在第 27 行附近。注意它记录了项目编码标准，但尚未加入代码内文档规则，这就是生成的 helper 没有文档注释的原因。
3. 在现有标准的正下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，说明文件既可以在存储库级别（`.github/copilot-instructions.md`）创建以提供全局指导，也可以创建为 `*.instructions.md` 文件，为特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会从议题目标分支读取说明。下一课将把议题分配给 Copilot，届时它会从 `main` 创建分支。因此，必须先将说明更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。这样，云智能体在下一课处理议题时便会读取这些说明。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。这里保留分支步骤是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何读取本项目中的说明文件，并向 `.github/copilot-instructions.md` 添加了一项新的全库标准。接下来几课中，云智能体创建的拉取请求会体现这项标准。

下一步，在[练习 2 - GitHub Copilot 云智能体][next-lesson]中分配第一个议题。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好的自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
