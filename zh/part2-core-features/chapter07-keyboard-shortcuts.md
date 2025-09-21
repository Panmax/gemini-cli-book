# 第七章：键盘快捷键与效率提升

到目前为止，我们已经探索了 Gemini CLI 的大部分核心功能。然而，要真正成为一名高效的使用者，掌握键盘快捷键和一些高级技巧是必不可少的。本章将为您介绍一系列能显著提升您工作效率的方法。

## 经验证的键盘快捷键

Gemini CLI 内置了许多方便的快捷键。这里是一份根据官方文档验证过的完整列表，以帮助您更快地编辑、提交和管理会话。

<br>

<table style="width:100%">
  <thead>
    <tr>
      <th style="width: 30%;">快捷键</th>
      <th>功能描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td colspan="2" style="text-align: center; background-color: #2d2d2d;"><strong>通用</strong></td>
    </tr>
    <tr>
      <td><strong><code>Esc</code></strong></td>
      <td>取消正在进行的模型生成或关闭对话框。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+C</code></strong></td>
      <td>取消当前请求。连续按两次可退出程序。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+D</code></strong></td>
      <td>退出程序 (如果输入框为空)。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+L</code></strong></td>
      <td>清屏。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+Y</code></strong></td>
      <td>切换 YOLO (You Only Live Once) 模式，该模式会自动批准工具调用。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+G</code></strong></td>
      <td>(IDE 集成) 查看从 IDE 接收到的上下文。</td>
    </tr>
    <tr>
      <td colspan="2" style="text-align: center; background-color: #2d2d2d;"><strong>输入与编辑</strong></td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+X</code></strong> / <strong><code>Meta+Enter</code></strong></td>
      <td>在外部编辑器中打开当前提示。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+V</code></strong></td>
      <td>粘贴剪贴板内容。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+A</code></strong> / <strong><code>Home</code></strong></td>
      <td>将光标移动到行首。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+E</code></strong> / <strong><code>End</code></strong></td>
      <td>将光标移动到行尾。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+左/右箭头</code></strong></td>
      <td>光标向前/后移动一个单词。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+W</code></strong> / <strong><code>Ctrl+Backspace</code></strong></td>
      <td>删除光标前的一个单词。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+U</code></strong></td>
      <td>删除从光标到行首的所有内容。</td>
    </tr>
    <tr>
      <td><strong><code>Ctrl+K</code></strong></td>
      <td>删除从光标到行尾的所有内容。</td>
    </tr>
    <tr>
      <td colspan="2" style="text-align: center; background-color: #2d2d2d;"><strong>导航与选择</strong></td>
    </tr>
    <tr>
      <td><strong><code>上/下箭头</code></strong></td>
      <td>在提示历史、建议或选择列表中上下导航。</td>
    </tr>
    <tr>
      <td><strong><code>Tab</code></strong> / <strong><code>Enter</code></strong></td>
      <td>接受一个选中的建议。</td>
    </tr>
    <tr>
      <td><strong><code>j</code></strong> / <strong><code>k</code></strong></td>
      <td>(在选择列表中) 向下/上移动选项。</td>
    </tr>
    <tr>
      <td><strong><code>1-9</code></strong></td>
      <td>(在选择列表中) 通过数字选择对应的项目。</td>
    </tr>
  </tbody>
</table>

## 解决快捷键冲突

由于 Gemini CLI 运行在您的终端应用（如 iTerm2, Windows Terminal, GNOME Terminal 等）之中，有时其内置快捷键可能会与终端自身的快捷键发生冲突。例如，某个终端工具可能已经将 `Ctrl+L` 映射为其他功能，导致在 Gemini CLI 中无法用它来清屏。

**解决方法：**

解决这个问题的最佳途径是在您的 **终端应用设置** 中进行调整，而不是在 Gemini CLI 中。因为 Gemini CLI 无法控制终端如何拦截键盘输入。

您需要做的是：
1.  **识别冲突：** 找到是哪个快捷键没有按预期在 Gemini CLI 中工作。
2.  **查找终端设置：** 打开您终端应用的偏好设置或选项，找到“快捷键”、“键位映射 (Keybindings)”或“Profiles”等相关部分。
3.  **修改或禁用冲突快捷键：** 在终端的设置中，找到那个冲突的快捷键组合，然后选择：
    *   **重新映射 (Remap):** 将该功能分配给一个新的、不常用的快捷键。
    *   **禁用 (Disable):** 如果这个终端快捷键对您不重要，可以直接禁用它。

通过将终端的快捷键“让位”给 Gemini CLI，您就可以确保所有 `gemini` 命令的内置快捷键都能正常工作了。

## 使用外部编辑器 (`Ctrl+X`) 编写复杂提示

当您需要编写一个包含大量代码、多行指令或复杂逻辑的提示时，终端中的单行输入框可能会显得有些局促。`Ctrl+X` 快捷键正是为了解决这个问题而设计的。

按下 `Ctrl+X` 后，Gemini CLI 会自动打开您系统默认的命令行编辑器（通常是 `vim` 或 `nano`）。您可以在这个功能齐全的编辑器中：
*   轻松地编写和编辑多行文本。
*   方便地粘贴和修改大段代码。
*   利用编辑器的语法高亮和搜索功能。

编辑完成后，保存并关闭编辑器，您在其中编写的所有内容都会被自动加载到 Gemini CLI 的输入框中，准备发送。

**实战场景：**
您需要让 Gemini CLI 帮你重构一个复杂的函数。
1.  在您的代码编辑器中复制这个函数的完整代码。
2.  回到 Gemini CLI，输入您的重构要求，例如：“请帮我重构以下函数，提高其可读性并添加错误处理：”。
3.  按下 `Ctrl+X`，进入外部编辑器。
4.  将您复制的函数代码粘贴到指令下方。
5.  保存并退出编辑器。
6.  您的完整、复杂的多行提示已经准备好，直接发送即可。

## 提升日常工作效率的技巧和窍门

1.  **善用 `GEMINI.md`:** 将项目中最常用、最核心的背景信息和编码规范放在 `GEMINI.md` 中。这是一个一劳永逸的投资，能极大提升后续所有回答的质量。

2.  **链式思考 (Chain of Thought):** 当面对一个复杂任务时，不要试图用一个庞大而复杂的提示一次性解决。而是应该像与真人结对编程一样，将任务分解成多个步骤，通过连续的对话引导 AI 一步步完成。
    *   **第一步：** “我们来分析一下 `@src/main.js` 的功能。”
    *   **第二步：** “好的，现在请为 `processData` 函数编写单元测试。”
    *   **第三步：** “测试覆盖率看起来不错，现在我们来重构这个函数，让它支持缓存。”

3.  **结合使用 `@` 和 `!`:** 将文件上下文和 shell 命令结合起来，可以创造出强大的工作流。
    ```
    > @src/database/schema.sql
    > 根据这个 SQL schema，使用 `touch` 命令在 `@src/models` 目录下创建对应的模型文件。
    ```
    在这个例子中，您让 AI 先读取 schema 文件，然后根据文件内容，智能地生成并执行一系列 `touch` 命令来创建文件。

4.  **利用 `/save` 和 `/resume` 创建任务模板:** 对于重复性任务，例如“为组件生成 Storybook 文件”，您可以创建一个包含初始指令的会话，并将其保存为 `storybook_template.json`。下次需要执行相同任务时，只需 `/resume storybook_template.json` 并提供新的组件文件 `@components/MyNewComponent.js` 即可快速开始。

通过将这些快捷键和高级技巧融入您的日常工作，您将能够更加流畅、高效地与 Gemini CLI 协作，真正发挥出 AI 辅助开发的巨大潜力。
