# 第九章：通过扩展来扩展 Gemini CLI 的能力

虽然 Gemini CLI 开箱即用时已经非常强大，但其真正的潜力需要通过一个现代化的、可分享的**扩展系统**来解锁。扩展允许您将提示、自定义命令、甚至复杂的 MCP 服务器打包成一种用户友好的格式，可以被轻松地安装和分享。

本章将聚焦于这种全新的、官方推荐的扩展 Gemini CLI 的方式。

## 通过命令行管理扩展

Gemini CLI 提供了一套通过 `gemini extensions` 命令使用的扩展管理工具。请注意，这些命令必须在您的系统 shell 中运行，而不是在 Gemini CLI 的交互式会话内部。

### 安装扩展
您可以使用 GitHub URL 或本地目录来安装扩展。这是为您的 CLI 添加新功能的主要方式。
```bash
# 从 GitHub 安装
gemini extensions install https://github.com/google-gemini/gemini-cli-security

# 从本地目录安装
gemini extensions install --path=path/to/local/extension
```

### 其他管理命令
- **卸载:** `gemini extensions uninstall <扩展名称>`
- **禁用/启用:** `gemini extensions disable <扩展名称>` (也可以配合 `--scope=workspace` 仅为当前项目禁用)。
- **更新:** `gemini extensions update <扩展名称>` 或 `gemini extensions update --all`。

## 扩展的工作原理

启动时，Gemini CLI 会在 `<home>/.gemini/extensions` 目录中寻找扩展。每个扩展都是一个文件夹，其中包含一个 `gemini-extension.json` 文件，该文件定义了扩展的行为。

### `gemini-extension.json` 文件
这个清单文件是扩展的核心。以下是一个示例结构：
```json
{
  "name": "my-extension",
  "version": "1.0.0",
  "mcpServers": {
    "my-server": {
      "command": "node my-server.js"
    }
  },
  "contextFileName": "GEMINI.md",
  "excludeTools": ["run_shell_command(rm -rf)"]
}
```
- **`name`**: 扩展的唯一名称。
- **`version`**: 扩展的版本。
- **`mcpServers`**: 定义并配置该扩展需要运行的任何 MCP 服务器。这是管理 MCP 服务器的现代方式，为用户抽象了其复杂性。
- **`contextFileName`**: 扩展内部的一个文件，为模型提供上下文，类似于项目级的 `GEMINI.md`。
- **`excludeTools`**: 允许扩展禁用某些工具以确保安全或相关性，例如，禁用 `rm -rf` 命令。

### 自定义命令
扩展可以通过在 `commands/` 子目录中放置 `.toml` 文件来提供自己的自定义斜杠命令。这是捆绑可复用提示和脚本的强大方式。

如果扩展中的自定义命令与用户自己的命令发生冲突，扩展的命令将被自动重命名。例如，一个扩展的 `/deploy` 命令可能会变为 `/my-extension.deploy` 以避免冲突。

## 创建您自己的扩展

Gemini CLI 让创建您自己的扩展变得非常简单。

### 创建一个样板扩展
您可以基于官方示例来生成一个新的扩展：
```bash
gemini extensions new path/to/your/new/extension custom-commands
```
该命令会创建一个新目录，其中包含一个预设好的 `gemini-extension.json` 和一个示例命令，为您提供一个完美的开发起点。

### 为开发链接扩展
为了简化开发流程，您可以使用 `link` 命令。它会创建一个从您的开发目录到 Gemini CLI 扩展文件夹的符号链接，这样您所做的任何更改都会立即生效，无需不断地更新或重装。
```bash
gemini extensions link path/to/your/dev/extension
```
通过利用这个扩展系统，您可以创建强大的、可复用的、可分享的工具，并将其深度集成到您特定的工作流中。
