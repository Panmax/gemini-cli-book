# Chapter 7: Keyboard Shortcuts and Efficiency Boosts

So far, we have explored most of the core features of Gemini CLI. However, to truly become an efficient user, mastering keyboard shortcuts and some advanced techniques is essential. This chapter will introduce a series of methods that can significantly improve your workflow efficiency.

## Common Keyboard Shortcuts

Gemini CLI has many convenient built-in shortcuts for its interactive session, which help you edit, submit, and manage your sessions more quickly.

| Shortcut | Description |
| :--- | :--- |
| **`Ctrl + L`** | Clears the screen, same as the `/clear` command, but faster. |
| **`Ctrl + D`** | Exits Gemini CLI, same as the `/quit` or `/exit` commands. |
| **`Ctrl + C`** | Interrupts the current operation, such as canceling an AI response that is being generated. |
| **`Ctrl + R`** | Searches through your command history. |
| **`Up/Down Arrows`** | Navigates through the history of prompts you have submitted. |
| **`Ctrl + V`** | **Safe Paste Mode**. This is a very useful feature when you need to paste multiple lines of code or text. It enters a special paste mode that allows you to preview and edit the pasted content before submitting, avoiding formatting issues or accidental submissions that can occur with a normal paste. |
| **`Ctrl + Y`** | **YOLO (You Only Live Once) Mode**. This is an advanced feature that automatically approves and executes the first tool call suggested by the model (e.g., a file write or shell command). This is a "trust mode" that can speed up your workflow by skipping the confirmation step when you are completely certain of the model's intent. **Use this feature with caution**, as it may modify files or execute commands without your explicit confirmation. |
| **`Ctrl + X`** | **Edit with an external editor**. We will discuss this powerful feature in detail in the next section. |

## Resolving Shortcut Conflicts

Since Gemini CLI runs inside your terminal application (like iTerm2, Windows Terminal, GNOME Terminal, etc.), its built-in shortcuts can sometimes conflict with the terminal's own shortcuts. For example, a terminal tool might already map `Ctrl+L` to another function, preventing it from clearing the screen in Gemini CLI.

**The Solution:**

The best way to resolve this is to make adjustments in your **terminal application's settings**, not within Gemini CLI. This is because Gemini CLI cannot control how the terminal intercepts keyboard inputs.

Here's what you need to do:
1.  **Identify the Conflict:** Find out which shortcut is not behaving as expected within Gemini CLI.
2.  **Locate Terminal Settings:** Open your terminal application's preferences or options and find the relevant section, such as "Keys," "Keybindings," or "Profiles."
3.  **Modify or Disable the Conflicting Shortcut:** In your terminal's settings, find the conflicting key combination and then choose to:
    *   **Remap:** Assign the terminal's feature to a new, less common shortcut.
    *   **Disable:** If the terminal shortcut is not important to you, simply disable it.

By "making way" for Gemini CLI's shortcuts in your terminal's settings, you can ensure that all of the `gemini` command's built-in shortcuts work correctly.

## Using an External Editor (`Ctrl+X`) for Complex Prompts

When you need to write a prompt that contains a large amount of code, multi-line instructions, or complex logic, the single-line input box in the terminal can feel restrictive. The `Ctrl+X` shortcut is designed to solve this problem.

After pressing `Ctrl+X`, Gemini CLI will automatically open your system's default command-line editor (usually `vim` or `nano`). In this full-featured editor, you can:
*   Easily write and edit multi-line text.
*   Conveniently paste and modify large blocks of code.
*   Utilize the editor's syntax highlighting and search features.

Once you are done editing, save and close the editor. All the content you wrote will be automatically loaded into the Gemini CLI input box, ready to be sent.

**Practical Scenario:**
You need Gemini CLI to help you refactor a complex function.
1.  Copy the entire code of the function from your code editor.
2.  Return to Gemini CLI and type your refactoring request, for example: "Please help me refactor the following function to improve its readability and add error handling:".
3.  Press `Ctrl+X` to enter the external editor.
4.  Paste the copied function code below your instruction.
5.  Save and exit the editor.
6.  Your complete, complex, multi-line prompt is now ready to be sent.

## Tips and Tricks for Boosting Daily Efficiency

1.  **Make Good Use of `GEMINI.md`:** Place the most common and core background information and coding standards for your project in `GEMINI.md`. This is a one-time investment that can greatly improve the quality of all subsequent responses.

2.  **Chain of Thought:** When facing a complex task, don't try to solve it all at once with a single, massive prompt. Instead, like pair programming with a human, break the task down into multiple steps and guide the AI through them with a series of continuous conversations.
    *   **Step 1:** "Let's analyze the functionality of `@src/main.js`."
    *   **Step 2:** "Okay, now please write unit tests for the `processData` function."
    *   **Step 3:** "The test coverage looks good. Now let's refactor this function to support caching."

3.  **Combine `@` and `!`:** Combining file context with shell commands can create powerful workflows.
    ```
    > @src/database/schema.sql
    > Based on this SQL schema, use the `touch` command to create the corresponding model files in the `@src/models` directory.
    ```
    In this example, you have the AI first read the schema file and then, based on its content, intelligently generate and execute a series of `touch` commands to create the files.

4.  **Use `/save` and `/resume` to Create Task Templates:** For repetitive tasks, such as "generating a Storybook file for a component," you can create a session with the initial instructions and save it as `storybook_template.json`. The next time you need to perform the same task, you can simply `/resume storybook_template.json` and provide the new component file `@components/MyNewComponent.js` to get started quickly.

By incorporating these shortcuts and advanced techniques into your daily work, you will be able to collaborate with Gemini CLI more smoothly and efficiently, truly unlocking the immense potential of AI-assisted development.
