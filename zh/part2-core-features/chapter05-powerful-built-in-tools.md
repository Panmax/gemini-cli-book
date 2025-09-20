# 第五章：强大的内置工具

Gemini CLI 的强大之处不仅在于它能理解和生成语言，更在于它被赋予了执行具体“动作”的能力。这些动作由一系列内置的“工具” (Tools) 提供，它们让 Gemini CLI 能够与您的本地环境和互联网进行交互，从而完成更复杂的任务。

模型会根据您的指令，智能地决定是否以及如何使用这些工具。在本章中，我们将探索几类最核心的内置工具。

## 文件系统工具

文件系统工具赋予了 Gemini CLI 直接读写本地文件的能力。这意味着 AI 可以像您一样，查看文件内容、创建新文件或修改现有文件。

在前面的章节中，当我们让 AI 生成 `server.js` 并保存时，它其实就是在后台调用了 `write-file` 这个工具。

### 常见文件工具

*   `read_file`: 读取指定文件的内容。
*   `write_file`: 将内容写入指定文件（如果文件已存在，则会覆盖）。
*   `list_directory`: 列出指定目录下的文件和子目录。

### 实战示例：代码重构

假设您有一个很长的函数，您希望将其拆分到多个文件中。

**您的提示：**
```
> 我觉得 @src/utils.js 这个文件太大了。
> 请把里面的 `networkRequest` 和 `dataProcessing` 函数抽离出来，
> 分别放到 @src/network.js 和 @src/data.js 两个新文件中，
> 并在原文件 (`utils.js`) 中删除这两个函数。
```

**Gemini CLI 的执行过程：**

1.  **智能规划:** 模型接收到指令后，会分析出这是一个多步骤的文件操作任务。
2.  **调用 `read_file`:** 首先，它会调用 `read_file` 工具来读取 `src/utils.js` 的完整内容，以确保它理解了要移动的函数。
3.  **调用 `write_file`:** 接着，它会两次调用 `write_file` 工具：
    *   第一次，创建 `src/network.js` 并将 `networkRequest` 函数的代码写入。
    *   第二次，创建 `src/data.js` 并将 `dataProcessing` 函数的代码写入。
4.  **再次调用 `write_file`:** 最后，它会再次调用 `write_file` 来更新 `src/utils.js`，将已经抽离出去的两个函数删除。
5.  **用户确认:** 在每一步关键的文件写入操作前，Gemini CLI 都会向您请求确认，确保一切都在您的掌控之中。

通过这种方式，Gemini CLI 将一个复杂的重构任务，分解为一系列对文件系统工具的原子调用，既自动化了流程，又保证了安全。

## Shell 工具

Shell 工具是一个非常强大的功能，它允许 Gemini CLI 直接在您的终端中执行 shell 命令。这极大地扩展了它的能力边界，让它可以完成安装依赖、运行测试、操作 Git 等各种系统级任务。

您可以通过在提示的行首添加 `!` 来手动触发 Shell 工具。

### 实战示例：自动化工作流

**示例一：安装依赖并启动项目**
```
> !npm install && npm run dev
```
这个命令会先安装项目依赖，然后启动开发服务器。

**示例二：创建新分支并提交代码**
```
> 我的代码修改已经完成了。
> !git checkout -b feature/new-login-page
> !git add .
> !git commit -m "feat: implement new login page"
```

**模型自动调用 Shell 工具**

更强大的是，模型可以根据您的自然语言指令，自动生成并执行 shell 命令。

**您的提示：**
```
> 请帮我检查一下项目中所有 TypeScript 文件的代码风格。
```

**Gemini CLI 的响应：**
```
好的，我将使用 `npx eslint . --ext .ts` 命令来检查 TypeScript 文件的代码风格。

[TOOL] Executing: `npx eslint . --ext .ts`
[?] Continue? (Y/n)
```
模型识别出您的意图，并自动生成了正确的 shell 命令。在执行前，它会清晰地展示将要运行的命令并请求您的批准。

## Web 获取工具

现代开发离不开海量网络信息的获取。Gemini CLI 内置了 Web 获取工具，使其具备实时访问互联网的能力。

*   `web_fetch`: 从一个或多个 URL 中获取内容。
*   `google_web_search`: 执行 Google 搜索并返回结果。

### 实战示例

**示例一：学习新技术**
```
> 我想学习如何在 React 中使用 Zustand 来进行状态管理。
> 请搜索相关的官方文档或优秀教程，并为我提供一个入门示例。
```
模型会调用 `google_web_search` 工具搜索 "React Zustand tutorial"，然后可能会找到 Zustand 在 GitHub 上的官方文档。接着，它会调用 `web_fetch` 工具来读取文档页面的内容，并最终根据这些信息，为您总结核心概念并生成一个入门代码示例。

**示例二：解决报错信息**
```
> 我的 Next.js 应用在启动时报了一个错：
> "Error: connect ECONNREFUSED 127.0.0.1:6379"
> 这通常是什么原因造成的？请帮我搜索一下解决方案。
```
模型会使用 `google_web_search` 工具搜索这个具体的错误信息，并根据搜索结果（很可能来自 Stack Overflow 或 GitHub Issues），为您分析出这通常是由于本地 Redis 服务未启动导致的，并给出启动 Redis 服务的建议命令。

通过将语言能力与这些强大的内置工具相结合，Gemini CLI 从一个单纯的“聊天机器人”转变为一个能够感知环境、执行操作、获取新知的“智能体” (Agent)，真正成为您开发工作流中的得力助手。
