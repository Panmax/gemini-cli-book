# Part 3: Advanced Features and Walkthroughs

Welcome to the third part of this book. In this section, we will go beyond the basics to explore some of the most powerful advanced features that Gemini CLI has to offer. These features will help you integrate AI more deeply and seamlessly into your daily development workflow to tackle more complex challenges.

---

# Chapter 8: Integrated Development Environment (IDE) Integration

While the command line is the core of Gemini CLI, we all know that modern developers spend the vast majority of their time in an Integrated Development Environment (IDE). To create a truly seamless AI-assisted experience, Gemini CLI offers deep integration with Visual Studio Code (VS Code).

This integration transforms Gemini CLI from a standalone terminal tool into a true programming partner that is aware of your IDE's state.

## The Advantages of VS Code Integration

Once IDE integration is enabled, Gemini CLI gains the following superpowers:

*   **Powerful Context-Awareness:** Gemini CLI can directly "see" what you are working on in VS Code. This context includes:
    *   The **10 most recently accessed files** in your workspace.
    *   Your **active cursor position**.
    *   Any **text you have selected** (up to a 16KB limit; longer selections will be truncated).

*   **Native IDE Diff View:** This is one of the most powerful features. When Gemini CLI suggests a modification to a file, it no longer just prints the code in the terminal. Instead, it opens a **native, full-screen diff view** in VS Code, allowing you to review, edit, and accept or reject changes seamlessly.

*   **VS Code Commands:** You can access key features directly from the VS Code Command Palette (`Cmd+Shift+P` or `Ctrl+Shift+P`), such as `Gemini CLI: Run` and `Gemini CLI: Accept Diff`.

## Installation and Activation Steps

There are three ways to set up the IDE integration.

**Prerequisites:**
*   Please run Gemini CLI in the **Integrated Terminal** of VS Code.

### 1. Automatic Prompt (Recommended)
The first time you run `gemini` in the VS Code integrated terminal, it will auto-detect the environment and prompt you to install the companion extension. Answering "Yes" is the easiest way to get started.

### 2. Manual Installation from the CLI
If you miss the prompt, you can run the following command in Gemini CLI to install it manually:
```
> /ide install
```

### 3. Manual Installation from a Marketplace
You can also install the "Gemini CLI Companion" extension directly from a marketplace.
- **For Visual Studio Code:** Install from the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=google.gemini-cli-vscode-ide-companion).
- **For VS Code Forks:** The extension is also published on the [Open VSX Registry](https://open-vsx.org/extension/google/gemini-cli-vscode-ide-companion).

**Important:** After manually installing the extension from a marketplace, you must run `/ide enable` in the CLI to activate the integration.

### Enabling and Disabling the Connection
You can manually control the connection status with the following commands:

*   **/ide enable:** Enables IDE integration.
*   **/ide disable:** Temporarily disables IDE integration.

Once enabled, a special icon will appear next to the Gemini CLI prompt, indicating that it has successfully connected to your VS Code instance.

## Post-Integration Workflow Example

Let's see how seamless your workflow can become after enabling integration.

1.  **Open Project:** Open your project in VS Code and open a file you want to modify, for example, `app.js`.
2.  **Open Integrated Terminal:** Use the `Ctrl+	` shortcut to open the VS Code integrated terminal.
3.  **Start Gemini CLI:** Type `gemini` to start an interactive session.
4.  **Make a Modification Request:** Now, you don't need to use `@app.js`. Gemini CLI already knows you are viewing this file. You can simply say:
    ```
    > Add a try-catch block to the handleRequest function to handle exceptions.
    ```
5.  **Review the Diff in the IDE:** Gemini CLI will open a diff view in VS Code, clearly showing the `try-catch` block it suggests.
6.  **Approve or Modify:** You can review the code in the view. If it looks good, just press `Cmd+S` / `Ctrl+S` to save the file, and the changes will be applied. If you need to make minor adjustments, you can edit the code directly in the right-hand window and then save.
7.  **Continue the Conversation:** Return to the terminal to continue with your next task.

In this way, you can complete the entire "dialogue -> code -> review -> apply" loop without ever leaving your IDE, greatly enhancing your development immersion and efficiency.

## Troubleshooting

If you encounter issues with IDE integration, here are some common error messages and how to resolve them.

### Connection Errors

- **Message:** `🔴 Disconnected: Failed to connect to IDE companion extension...`
  - **Cause:** The CLI could not connect to the IDE extension. This usually means the extension is not running or did not initialize correctly.
  - **Solution:** Ensure you have the **Gemini CLI Companion** extension installed and enabled. Try opening a new integrated terminal window in your IDE to ensure it picks up the correct environment.

- **Message:** `🔴 Disconnected: IDE connection error. The connection was lost unexpectedly...`
  - **Cause:** The connection to the IDE companion was lost.
  - **Solution:** Run `/ide enable` to try and reconnect. If the issue persists, open a new terminal window or restart your IDE.

### Configuration Errors

- **Message:** `🔴 Disconnected: Directory mismatch...`
  - **Cause:** The CLI's current working directory is outside the workspace you have open in your IDE.
  - **Solution:** `cd` into the same directory that is open in your IDE and restart the CLI.

- **Message:** `🔴 Disconnected: To use this feature, please open a workspace folder in [IDE Name] and try again.`
  - **Cause:** You have no workspace open in your IDE.
  - **Solution:** Open a workspace folder in your IDE and restart the CLI.
