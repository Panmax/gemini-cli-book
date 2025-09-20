# Part 3: Advanced Features and Walkthroughs

Welcome to the third part of this book. In this section, we will go beyond the basics to explore some of the most powerful advanced features that Gemini CLI has to offer. These features will help you integrate AI more deeply and seamlessly into your daily development workflow to tackle more complex challenges.

---

# Chapter 8: Integrated Development Environment (IDE) Integration

While the command line is the core of Gemini CLI, we all know that modern developers spend the vast majority of their time in an Integrated Development Environment (IDE). To create a truly seamless AI-assisted experience, Gemini CLI offers deep integration with Visual Studio Code (VS Code).

This integration transforms Gemini CLI from a standalone terminal tool into a true programming partner that is aware of your IDE's state.

## The Advantages of VS Code Integration

Once IDE integration is enabled, Gemini CLI gains the following superpowers:

*   **Powerful Context-Awareness:** Gemini CLI can directly "see" what you are working on in VS Code. This includes:
    *   **Currently Open Files:** It knows which file you are editing and can automatically use it as context, without you needing to manually reference it with `@`.
    *   **Selected Code:** When you select a piece of code in the editor, your next query will automatically focus on that code, which is perfect for local refactoring or code explanation.

*   **Native IDE Diff View:** This is one of the most powerful features. When Gemini CLI suggests a modification to a file, it no longer just prints the code in the terminal. Instead, it opens a **native, full-screen diff view** in VS Code. In this view, you can:
    *   Clearly see the code changes in red (deletions) and green (additions).
    *   Directly edit and revise the AI-suggested code in the diff view before approving the changes.
    *   Accept the changes with a single click of a button or a keyboard shortcut (`Cmd+S` / `Ctrl+S`), applying the modifications to your file.

This "what you see is what you get" interaction makes the process of reviewing and approving code changes both intuitive and safe, giving you complete control.

## Installation and Activation Steps

The integration process is very simple and usually only requires one or two commands.

**Prerequisites:**
*   Please run Gemini CLI in the **Integrated Terminal** of VS Code.
*   Ensure your Gemini CLI version is at least `0.1.20` and your VS Code version is at least `1.99.0`.

### Step 1: Install the Companion Extension

You need to install a VS Code extension called "Gemini CLI Companion." There are two ways to install it:

**1. Automatic Installation (Recommended):**
The first time you run `gemini` in the VS Code integrated terminal, it will usually auto-detect the environment and give you a prompt:
```
[INFO] Gemini CLI can integrate with VS Code for a better experience.
[?] Install the companion VS Code extension to enable this? (Y/n)
```
Simply type `Y` and press Enter, and Gemini CLI will automatically install and configure the companion extension for you.

**2. Manual Installation:**
If for some reason you miss the automatic prompt, you can run the following command in Gemini CLI to install it manually:
```
> /ide install
```
Alternatively, you can search for "Gemini CLI Companion" in the VS Code Extensions Marketplace and click install.

### Step 2: Enable Integration

After installing the extension, integration is enabled by default. You can manually control its status with the following commands:

*   **/ide enable:** Enables IDE integration.
    ```
    > /ide enable
    ```
*   **/ide disable:** Temporarily disables IDE integration.
    ```
    > /ide disable
    ```

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
