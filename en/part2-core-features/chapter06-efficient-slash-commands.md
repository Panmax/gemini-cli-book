# Chapter 6: Efficient Slash (/) Commands

In addition to conversing with the Gemini model through prompts, Gemini CLI also provides a powerful set of "Slash Commands." These commands, which begin with a `/`, are used to manage sessions, control context, configure tools, and perform other meta-operations. They are key to boosting your efficiency within Gemini CLI.

You can type `/` at the `> ` prompt in an interactive session to see all available commands. This chapter will detail the most common and important ones.

## Common Basic Commands

These are the commands you will interact with most frequently in your daily use.

*   **/help:** Displays help information, listing all available slash commands and their brief descriptions. This is your go-to command when you forget a command.
    ```
    > /help
    ```

*   **/clear:** Clears the entire history of the current session, including the prompts you've sent and the model's responses. This gives you a clean interface, but be aware that the previous conversation context will also be cleared.
    ```
    > /clear
    ```

*   **/stats:** Shows statistics for the current session, such as the number of conversation turns and the amount of tokens used. This is helpful for understanding your usage and costs.
    ```
    > /stats
    ```

*   **/tools:** Lists all the built-in tools available in the current session, such as `ReadFile`, `GoogleSearch`, etc.
    ```
    > /tools
    ```

## Session and State Management

Gemini CLI provides powerful commands to manage not just your conversation history, but also the state of your project files, giving you a safety net for AI-driven modifications.

### Managing Chat Sessions (`/chat` commands)

The modern Gemini CLI uses a tag-based system to save and restore chat sessions. This is an evolution from older versions that saved sessions as `.json` files directly in your project directory. The new approach keeps your workspace clean by managing these snapshots internally, allowing you to reference them with a simple tag instead of a filename. This is extremely useful for creating checkpoints in a complex task.

*   **/chat save <tag>:** Saves the current conversation history under a specific, memorable tag.
    ```
    > /chat save refactor-in-progress
    ```
*   **/chat resume <tag>:** Restores the conversation history from a previously saved tag.
    ```
    > /chat resume refactor-in-progress
    ```
*   **/chat list:** Lists all the tags of your saved chat sessions.
    ```
    > /chat list
    ```
*   **/chat delete <tag>:** Deletes a saved chat session.
    ```
    > /chat delete refactor-in-progress
    ```
This tag-based system is ideal for pausing and resuming complex tasks or creating reusable workflow templates without cluttering your file system.

#### Practical Example: Creating a Reusable Workflow Template
Let's say you frequently need to generate Jest unit tests for your JavaScript functions. You can create a template for this task.

**Step 1: Create the Template Session**
Start a new `gemini` session and provide your initial instructions that define the task and the role of the AI.
```
> You are a testing expert specializing in Jest. I will provide you with a JavaScript function by referencing a file. Your task is to generate a complete Jest unit test file for it. The test should cover the main use case and at least one edge case. Please wait for me to provide the file.
```

**Step 2: Save the Template**
Now, save this initial context with a memorable tag.
```
> /chat save jest-template
```
The template is now saved. You can exit the session.

**Step 3: Use the Template**
Later, when you need to write a test for a new function, you can simply resume the template.
```
> /chat resume jest-template
```
The CLI will restore your initial instructions. Now, all you need to do is provide the specific file for the task at hand.
```
> Okay, here is the function: @src/utils/calculator.js
```
Gemini CLI will then proceed to generate a Jest test for `calculator.js`, perfectly following the rules you defined in your template.

### Undoing File Changes (`/restore`)

This is a powerful safety feature that automatically saves a snapshot of your project's state **before** any AI-powered tool modifies your files. This allows you to safely experiment with code changes, knowing you can instantly revert them.

> **Note:** This feature is disabled by default. You must enable it by either starting the CLI with the `--checkpointing` flag, or by setting the following in your `settings.json` file:
> ```json
> {
>   "general": {
>     "checkpointing": {
>       "enabled": true
>     }
>   }
> }
> ```

Once enabled, a "checkpoint" is automatically created whenever you approve a tool that modifies the file system (like `write_file`).

*   **/restore:** When run without arguments, it lists all available checkpoints for the current project.
*   **/restore <checkpoint_file>:** Reverts all files in your project to the state captured in the specified checkpoint and restores the conversation history up to that point.

## Context Management (`/memory` commands)

Effectively managing context is key to collaborating efficiently with the AI. The `/memory` command and its sub-commands allow you to precisely view and control the context information.

*   **/memory show:** This is a very useful debugging command. It displays the full context that will be sent to the model on your **next** prompt submission. This includes system instructions (from `GEMINI.md`), file content, and conversation history.
    ```
    > @package.json
    > /memory show
    ```
    This helps you confirm whether the AI is "seeing" the information you want it to see.

*   **/memory refresh:** Reloads the system instructions from all `GEMINI.md` files. This is very useful when you have modified a context file during a session and want the changes to take effect immediately without restarting.
    ```
    > /memory refresh
    ```

## Other Useful Commands

*   **/theme:** Opens an interactive menu that allows you to select or customize the interface's color theme.
    ```
    > /theme
    ```

*   **/quit** or **/exit:** Exits the Gemini CLI interactive session. You can also use the shortcut `Ctrl+D`.

Mastering these slash commands will give you much greater control over Gemini CLI and significantly improve your workflow efficiency. It is recommended that you try them out in practice to experience the convenience they offer.
