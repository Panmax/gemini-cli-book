# 第三章：您的第一个交互式会话 (REPL)

在成功安装和配置 Gemini CLI 之后，现在是时候启动它并开始您的第一次 AI 编程对话了。本章将带您熟悉 Gemini CLI 的核心——交互式会话，也称为 REPL (Read-Eval-Print Loop)。

## 启动交互式模式

交互式模式是 Gemini CLI 最常用、最强大的功能。它提供了一个类似聊天应用的界面，让您可以与 Gemini 模型进行持续的对话。

要启动交互式模式，只需在您的终端中输入 `gemini` 命令：

```bash
gemini
```

按下回车后，您会看到一个欢迎界面，以及一个等待您输入的提示符 `> `。这表示 Gemini CLI 已经准备好接收您的指令了。

```
Welcome to Gemini CLI!

Ask me anything, or type /help for a list of commands.
>
```

## 编写您的第一个提示 (Prompt)

“提示 (Prompt)” 是您向 Gemini CLI 发出的指令或问题。一个好的提示是获得高质量回答的关键。让我们从一个简单的任务开始。

在 `> ` 提示符后，输入以下内容，然后按两次回车键（一次用于换行，一次用于发送）：

```
> 写一个 Python 函数，用于计算两个数字的和。
```

发送提示后，Gemini CLI 会连接到 Gemini 模型进行处理，很快您就会看到类似下面的响应。

## 理解 Gemini CLI 的响应和输出

Gemini CLI 的响应通常包含由 AI 生成的文本和代码。对于我们上面的提示，响应可能如下所示：

```
当然，这是一个简单的 Python 函数，用于计算两个数字的和：

```python
def add_numbers(a, b):
  """
  This function takes two numbers as input and returns their sum.
  """
  return a + b

# 示例用法
result = add_numbers(5, 3)
print(f"The sum is: {result}")
```

这个响应清晰地展示了几个关键部分：
*   **解释性文本:** AI 首先会用自然语言解释它将要做什么。
*   **代码块:** 代码会以语法高亮的形式呈现在一个格式化的块中，易于阅读和复制。
*   **示例:** 通常会附带一个简单的使用示例，告诉您如何调用生成的代码。

## 非交互式模式

除了交互式的 REPL，Gemini CLI 也支持非交互式模式。当您希望快速获取答案而无需进入完整会话时，这种模式非常有用，也便于在脚本中调用。

您可以结合使用 `-p` (或 `--prompt`) 参数和您的提示内容。

```bash
gemini -p "写一个 Python 函数，用于计算两个数字的和。"
```

执行该命令后，Gemini CLI 会直接在终端中打印响应，然后退出，不会进入交互式模式。

## 实战演练：生成一个简单的 "Hello, World!" 应用

现在，让我们通过一个更贴近实际开发的例子，来体验一下 Gemini CLI 的能力。我们将让它为我们创建一个简单的 Node.js "Hello, World!" Web 服务器。

**第一步：提出我们的需求**

在 Gemini CLI 的交互式会话中，输入以下提示：

```
> 我需要一个使用 Node.js 编写的简单 Web 服务器。
> 它应该在 3000 端口上监听请求。
> 当收到请求时，它应该返回 "Hello, World!"。
> 请将所有代码放在一个名为 `server.js` 的文件中。
```

**第二步：审查并应用 AI 生成的代码**

Gemini CLI 会理解您的多行指令，并生成相应的代码。它甚至会识别出您想要创建一个文件，并询问您是否要执行这个操作。

```
好的，这是一个满足您需求的 Node.js Web 服务器代码。

```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello, World!\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

[INFO] The model has suggested a file change.
[?] Do you want to apply this change to server.js? (Y/n)
```

这里，Gemini CLI 识别出代码应该被写入 `server.js`，并提示您确认。按下 `Y` 然后回车。

**第三步：运行应用**

现在，在您的项目目录下已经有了一个 `server.js` 文件。您可以使用 `!` 前缀在 Gemini CLI 中直接运行 shell 命令来启动这个服务器：

```
> !node server.js
```

您会看到输出 `Server running at http://127.0.0.1:3000/`。现在打开您的浏览器访问这个地址，或者在另一个终端中使用 `curl` 命令，您将看到 "Hello, World!" 的响应。

通过这个简单的实战，您已经体验了从提出需求、生成代码、创建文件到执行测试的完整流程。在后续的章节中，我们将探索更多高级功能，让您能够应对更复杂的开发任务。
