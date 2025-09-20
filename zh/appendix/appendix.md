# 附录

本节包含一些作为参考很有用，但不在本书核心学习路径之内的补充信息。

---

## 附录 A：配额与定价

Gemini CLI 提供了慷慨的免费使用额度，但对于更重度的使用场景，了解其配额和定价策略非常重要。这些策略根据您的认证方式而有所不同。

### 免费使用套餐
- **通过 Google 登录 (个人版 Gemini Code Assist):** 您的使用包含在一个免费套餐中，拥有可观的每日和每分钟请求限制，可使用一系列 Gemini 模型。
- **通过 Gemini API 密钥登录 (未付费):** API 密钥的免费套餐请求限制较低，且通常仅限于使用 Flash 模型。
- **通过 Vertex AI 登录 (Express 模式):** 提供 90 天的试用期，无需启用计费。

### 付费套餐
- **固定价格 (标准版/企业版 Gemini Code Assist):** 如果您需要更高且可预测的限额，可以订阅付费的 Gemini Code Assist 计划。这为每位用户提供了更高的每日请求次数，并按月收取固定费用。
- **按量付费 (Gemini API 或 Vertex AI):** 为了获得最大的灵活性并避免被速率限制，您可以使用启用了计费的 Gemini API 密钥或 Vertex AI。在这种模式下，您按使用的 token 数量付费。这是专业或自动化用例的理想选择，但需要您留意使用量以避免意外开销。

您可以随时使用 `/stats` 命令查看当前会话的使用情况。

---

## 附录 B：遥测与数据隐私

了解您的数据如何被处理至关重要。本节将介绍 CLI 的遥测功能以及适用于您代码的隐私政策。

### 遥测 (可观测性)
Gemini CLI 包含一个基于行业标准 OpenTelemetry 框架的可选遥测功能。启用后，它可以收集关于 CLI 使用情况、性能和错误的匿名数据。这有助于开发团队改进工具。

此功能**默认禁用**。您可以通过编辑 `settings.json` 文件来启用它，并将其配置为将数据发送到 Google Cloud 或本地文件以供您自己分析。

### 隐私与您的代码使用
任何开发者最关心的问题是：**“我的代码会被用来训练 Google 的模型吗？”** 答案完全取决于您的认证方式。

- **是，如果您的代码可能会被用于训练，如果您通过以下方式登录：**
  - **个人 Google 账户** (个人版 Gemini Code Assist)。
  - 使用**未付费套餐**的 Gemini API 密钥。

- **不，您的代码被视为机密，不会被用于训练，如果您通过以下方式登录：**
  - **Google Workspace 账户** (标准版或企业版 Gemini Code Assist)。
  - 使用**付费套餐**的 Gemini API 密钥。
  - **Google Cloud (Vertex AI)** 账户。

对于企业和专业用户，您的数据是保密的。对于免费套餐的个人用户，您的数据可能会被用来改进服务。您可以在 Google 的官方文档页面上找到适用于您账户类型的具体服务条款和隐私声明。

---

## 附录 C：卸载 Gemini CLI

如何卸载 CLI 取决于您的安装方式。

### 如果您通过 `npm install -g` 安装
这是最常见的方法。使用带 `-g` 标志的 npm uninstall 命令。
```bash
npm uninstall -g @google/gemini-cli
```

### 如果您使用 `npx`
`npx` 从一个临时缓存中运行包，而不会永久安装。要“卸载”它，您需要清除这个缓存。

- **在 macOS / Linux 上:**
  ```bash
  rm -rf "$(npm config get cache)/_npx"
  ```
- **在 Windows (PowerShell) 上:**
  ```powershell
  Remove-Item -Path (Join-Path $env:LocalAppData "npm-cache\_npx") -Recurse -Force
  ```
请注意，这将删除所有之前通过 `npx` 运行的包，而不仅仅是 `gemini-cli`。
