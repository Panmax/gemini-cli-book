# 第九章：扩展 Gemini CLI 的能力

Gemini CLI 的设计哲学是开放和可扩展的。除了强大的内置工具外，它还提供了一套先进的机制，允许您连接外部工具、服务和数据源，甚至创建完全自定义的命令。这套机制的核心就是**模型上下文协议 (Model Context Protocol, MCP)**。

本章将带您了解 MCP 的概念，并指导您如何通过构建一个简单的 MCP 服务器来为您自己的 Gemini CLI 添加独一無二的功能。

## 什么是模型上下文协议 (MCP)？

您可以将 MCP 想象成一座桥梁。它是一种标准化的协议，允许 Gemini CLI (客户端) 与外部工具提供方 (服务器) 进行通信。

通过这座桥梁，Gemini CLI 可以：
*   **发现工具:** 询问服务器：“你有哪些可用的工具？它们的功能是什么？需要哪些参数？”
*   **执行工具:** 请求服务器：“请帮我运行这个名为 `get_weather` 的工具，参数是 `location: "北京"`。”
*   **获取响应:** 接收服务器返回的结构化数据（例如，天气信息）。

这意味着，您可以编写一个小型的服务器应用，将您自己的脚本、公司的内部 API 或任何第三方服务，封装成 Gemini CLI 可以理解和调用的“工具”。

## 通过 MCP 连接外部工具

让我们通过一个实战案例来理解这个过程。我们将创建一个自定义工具 `/weather`，它能获取指定城市的实时天气。

### 第一步：创建 MCP 服务器

我们将使用 Node.js 和 Express 框架来创建一个简单的 MCP 服务器。这个服务器只做一件事：暴露一个 `/mcp` 端点，用于与 Gemini CLI 通信。

**`mcp-weather-server.js`**
```javascript
const express = require('express');
const app = express();
const port = 3001;

// 模拟一个调用天气 API 的函数
async function fetchWeather(location) {
  // 在真实应用中，这里会调用一个真实的天气 API
  if (location.includes('北京')) {
    return { temperature: '15°C', condition: '晴' };
  }
  return { temperature: '20°C', condition: '多云' };
}

app.use(express.json());

// MCP 端点
app.all('/mcp', async (req, res) => {
  const body = req.body;

  // 1. 工具发现 (Discovery)
  if (body.type === 'discovery') {
    return res.json({
      // 声明我们提供一个名为 'weather' 的工具集
      toolsets: [{ name: 'weather' }]
    });
  }

  // 2. 工具描述 (Describe)
  if (body.type === 'describe' && body.toolset === 'weather') {
    return res.json({
      // 描述工具集的具体工具
      tools: [{
        name: 'get_weather',
        description: '获取指定城市的实时天气',
        // 定义工具需要的参数
        parameters: [{
          name: 'location',
          type: 'STRING',
          description: '城市名称'
        }]
      }]
    });
  }

  // 3. 工具执行 (Execute)
  if (body.type === 'execute' && body.tool?.name === 'get_weather') {
    const location = body.tool.parameters.find(p => p.name === 'location')?.value;
    if (!location) {
      return res.status(400).json({ error: 'Location parameter is required' });
    }
    const weatherData = await fetchWeather(location);
    // 返回结构化的执行结果
    return res.json({
      tool: body.tool,
      result: JSON.stringify(weatherData)
    });
  }

  // 默认或未知请求
  res.status(404).send('Not Found');
});

app.listen(port, () => {
  console.log(`MCP Weather Server listening at http://localhost:${port}`);
});
```
这个服务器实现了 MCP 协议的三个核心部分：发现、描述和执行。

### 第二步：配置和管理 MCP 服务器

现在，我们需要告诉 Gemini CLI 如何找到我们的天气服务器。这通过修改 `settings.json` 配置文件来完成。

1.  运行 `gemini config path` 找到您的 `settings.json` 文件并打开它。
2.  在文件中添加 `mcpServers` 配置块：

**`settings.json`**
```json
{
  // ... 其他配置 ...
  "mcpServers": {
    "weatherServer": {
      "transport": "http",
      "url": "http://localhost:3001/mcp",
      "trusted": true
    }
  }
}
```
*   `weatherServer`: 这是您为服务器起的一个名字。
*   `transport`: 通信方式，这里是 `http`。
*   `url`: 我们服务器的地址。
*   `trusted`: 设置为 `true` 可以让 Gemini CLI 在调用工具时跳过烦人的确认步骤，因为这是我们自己开发的、可信的工具。

### 第三步：启动服务器并使用新工具

1.  **启动服务器:** 在终端中运行我们的服务器脚本。
    ```bash
    node mcp-weather-server.js
    ```
    您应该会看到 `MCP Weather Server listening at http://localhost:3001` 的输出。

2.  **启动 Gemini CLI:** 在**另一个**终端中，启动 `gemini`。它会自动读取配置并连接到您的 MCP 服务器。

3.  **使用自定义工具:** 现在，您可以像使用内置工具一样，通过自然语言来调用我们自定义的天气工具了！

    **您的提示：**
    ```
    > 北京今天天气怎么样？
    ```

    **Gemini CLI 的响应：**
    Gemini 模型会理解您的意图，并发现您本地的 `get_weather` 工具可以满足这个需求。于是，它会自动调用这个工具。
    ```
    [TOOL] Calling: get_weather({ location: '北京' })
    [TOOL] Result: {"temperature":"15°C","condition":"晴"}

    北京今天的天气是晴天，温度为 15°C。
    ```

通过这个简单的例子，您已经掌握了扩展 Gemini CLI 的核心方法。您可以举一反三，创建更复杂的工具来连接您的数据库、操作您的 CI/CD 流水线、或者集成任何您能想到的 API，将 Gemini CLI 打造成一个真正属于您的、无所不能的开发中枢。
