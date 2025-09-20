# 第二章：安装与配置

在本章中，我们将引导您完成 Gemini CLI 的安装和基本配置过程。这个过程非常简单，只需几个步骤，您就可以开始您的 AI 编码之旅。

## 环境要求

在安装 Gemini CLI 之前，请确保您的开发环境满足以下基本要求：

*   **Node.js:** 您需要安装 Node.js **v18 或更高版本**。Gemini CLI 是基于 Node.js 构建的，因此这是唯一的硬性要求。您可以在终端中运行以下命令来检查您当前的 Node.js 版本：
    ```bash
    node -v
    ```
    如果您的版本低于 v18，或者您尚未安装 Node.js，请访问 [Node.js 官方网站](https://nodejs.org/) 下载并安装最新版本。我们推荐使用 `nvm` (Node Version Manager) 来管理多个 Node.js 版本，这能让您在不同项目间轻松切换。

*   **操作系统:** Gemini CLI 跨平台兼容，支持 macOS, Windows 和 Linux。

## 安装步骤

您可以根据自己的喜好选择两种主要的安装方式：全局安装或通过 `npx` 快速运行。

### 方式一：全局安装 (推荐)

全局安装会将 `gemini` 命令注册到您的系统路径中，让您可以从任何位置方便地启动它。这是最推荐的方式。

打开您的终端，运行以下命令：

```bash
npm install -g @google/gemini-cli
```

`npm` 是 Node.js 的包管理器，通常会随 Node.js 一起安装。`-g` 标志告诉 `npm` 将这个包安装为全局可用。

安装完成后，您可以通过运行以下命令来验证安装是否成功：

```bash
gemini --version
```

如果终端输出了版本号，那么恭喜您，Gemini CLI 已经成功安装！

### 方式二：使用 `npx` 快速运行

如果您不想进行全局安装，或者只是想临时试用一下，您可以使用 `npx`。`npx` 是一个能让您直接运行 npm 包而无需先安装的工具。

在终端中运行：

```bash
npx @google/gemini-cli
```

这会下载最新版本的 Gemini CLI 并立即启动它。请注意，每次使用 `npx` 都会重新下载，因此对于日常使用来说，全局安装是更高效的选择。

## 认证方式

首次运行 Gemini CLI 时，它需要获取访问 Google Gemini API 的权限。目前支持两种认证方式。

### Google 账号登录 (默认)

当您第一次运行 `gemini` 命令时，它会自动打开您的默认浏览器，并引导您完成 Google 账号的登录和授权流程。

1.  在终端输入 `gemini`。
2.  浏览器会自动打开一个 Google 登录页面。
3.  选择您的 Google 账号并登录。
4.  在授权页面，点击“允许”，授予 Gemini CLI 访问所需 API 的权限。
5.  完成后，浏览器会显示一个成功页面，您就可以回到终端开始使用了。

这种方式非常便捷，推荐初学者使用。

### API 密钥配置

对于高级用户或在无法使用浏览器登录的环境中（例如 CI/CD 服务器），您可以通过配置 API 密钥来进行认证。

1.  访问 [Google AI Studio](https://aistudio.google.com/app/apikey) 获取您的 API 密钥。
2.  获取密钥后，通过以下命令将其设置到 Gemini CLI 中：
    ```bash
    gemini config set GOOGLE_API_KEY "YOUR_API_KEY"
    ```
    请将 `YOUR_API_KEY` 替换为您自己的密钥。

## 配置文件 (`settings.json`) 详解

Gemini CLI 会将您的设置保存在一个名为 `settings.json` 的文件中。您可以通过运行以下命令找到这个文件的具体位置：

```bash
gemini config path
```

这个 JSON 文件包含了您的认证信息、偏好设置等。其中一个常见的自定义项是主题。

### 个性化主题设置

Gemini CLI 支持自定义界面主题。您可以通过 `config set` 命令来修改主题颜色。例如，如果您想将主色调（primary）设置为蓝色，强调色（secondary）设置为紫色，可以运行：

```bash
gemini config set theme.primary "blue"
gemini config set theme.secondary "purple"
```

您可以直接编辑 `settings.json` 文件来进行更复杂的颜色定制，支持十六进制颜色码，例如：

```json
{
  "theme": {
    "primary": "#8A2BE2",
    "secondary": "#FF8C00",
    "danger": "#DC143C",
    "success": "#00FA9A"
  }
}
```

完成以上步骤后，您的 Gemini CLI 就已经完全配置好并准备就绪了。在下一章中，我们将正式开始我们的第一个交互式会话。
