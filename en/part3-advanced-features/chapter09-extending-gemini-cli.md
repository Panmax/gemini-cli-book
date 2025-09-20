# Chapter 9: Extending the Power of Gemini CLI

The design philosophy of Gemini CLI is open and extensible. In addition to its powerful built-in tools, it provides an advanced mechanism that allows you to connect to external tools, services, and data sources, and even create entirely custom commands. The core of this mechanism is the **Model Context Protocol (MCP)**.

This chapter will introduce you to the concept of MCP and guide you on how to add unique functionality to your own Gemini CLI by building a simple MCP server.

## What is the Model Context Protocol (MCP)?

You can think of MCP as a bridge. It is a standardized protocol that allows Gemini CLI (the client) to communicate with external tool providers (the servers).

Through this bridge, Gemini CLI can:
*   **Discover Tools:** Ask a server, "What tools do you have available? What do they do? What parameters do they need?"
*   **Execute Tools:** Request a server, "Please run the tool named `get_weather` with the parameter `location: "Beijing"`."
*   **Receive Responses:** Get structured data back from the server (e.g., weather information).

This means you can write a small server application to wrap your own scripts, your company's internal APIs, or any third-party service into "tools" that Gemini CLI can understand and call.

## Connecting to External Tools via MCP

Let's walk through a practical example to understand this process. We will create a custom tool, `/weather`, that can fetch the real-time weather for a specified city.

### Step 1: Create the MCP Server

We will use Node.js and the Express framework to create a simple MCP server. This server does just one thing: it exposes an `/mcp` endpoint for communicating with Gemini CLI.

**`mcp-weather-server.js`**
```javascript
const express = require('express');
const app = express();
const port = 3001;

// Simulate a function that calls a weather API
async function fetchWeather(location) {
  // In a real application, this would call a real weather API
  if (location.includes('Beijing')) {
    return { temperature: '15°C', condition: 'Sunny' };
  }
  return { temperature: '20°C', condition: 'Cloudy' };
}

app.use(express.json());

// The MCP endpoint
app.all('/mcp', async (req, res) => {
  const body = req.body;

  // 1. Tool Discovery
  if (body.type === 'discovery') {
    return res.json({
      // Declare that we provide a toolset named 'weather'
      toolsets: [{ name: 'weather' }]
    });
  }

  // 2. Tool Description
  if (body.type === 'describe' && body.toolset === 'weather') {
    return res.json({
      // Describe the specific tools in the toolset
      tools: [{
        name: 'get_weather',
        description: 'Get the real-time weather for a specified city',
        // Define the parameters the tool needs
        parameters: [{
          name: 'location',
          type: 'STRING',
          description: 'The name of the city'
        }]
      }]
    });
  }

  // 3. Tool Execution
  if (body.type === 'execute' && body.tool?.name === 'get_weather') {
    const location = body.tool.parameters.find(p => p.name === 'location')?.value;
    if (!location) {
      return res.status(400).json({ error: 'Location parameter is required' });
    }
    const weatherData = await fetchWeather(location);
    // Return the structured result
    return res.json({
      tool: body.tool,
      result: JSON.stringify(weatherData)
    });
  }

  // Default or unknown requests
  res.status(404).send('Not Found');
});

app.listen(port, () => {
  console.log(`MCP Weather Server listening at http://localhost:${port}`);
});
```
This server implements the three core parts of the MCP protocol: discovery, description, and execution.

### Step 2: Configure and Manage the MCP Server

Now, we need to tell Gemini CLI how to find our weather server. This is done by modifying the `settings.json` configuration file.

1.  Run `gemini config path` to find your `settings.json` file and open it.
2.  Add the `mcpServers` configuration block to the file:

**`settings.json`**
```json
{
  // ... other configurations ...
  "mcpServers": {
    "weatherServer": {
      "transport": "http",
      "url": "http://localhost:3001/mcp",
      "trusted": true
    }
  }
}
```
*   `weatherServer`: This is a name you give to your server.
*   `transport`: The communication method, which is `http` here.
*   `url`: The address of our server.
*   `trusted`: Setting this to `true` allows Gemini CLI to skip the confirmation step when calling the tool, as this is a trusted tool we developed ourselves.

### Step 3: Start the Server and Use the New Tool

1.  **Start the server:** Run our server script in the terminal.
    ```bash
    node mcp-weather-server.js
    ```
    You should see the output `MCP Weather Server listening at http://localhost:3001`.

2.  **Start Gemini CLI:** In **another** terminal, start `gemini`. It will automatically read the configuration and connect to your MCP server.

3.  **Use the custom tool:** Now, you can call our custom weather tool using natural language, just like a built-in tool!

    **Your Prompt:**
    ```
    > What's the weather like in Beijing today?
    ```

    **Gemini CLI's Response:**
    The Gemini model will understand your intent and discover that your local `get_weather` tool can fulfill this request. It will then automatically call the tool.
    ```
    [TOOL] Calling: get_weather({ location: 'Beijing' })
    [TOOL] Result: {"temperature":"15°C","condition":"Sunny"}

    The weather in Beijing today is sunny with a temperature of 15°C.
    ```

With this simple example, you have mastered the core method of extending Gemini CLI. You can apply this knowledge to create more complex tools to connect to your databases, operate your CI/CD pipelines, or integrate any API you can think of, turning Gemini CLI into a truly powerful and personalized development hub.
