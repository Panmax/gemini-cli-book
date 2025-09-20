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

*   **/tools:** Lists all the built-in tools available in the current session, such as `read_file`, `google_web_search`, etc.
    ```
    > /tools
    ```

## Session and State Management

Gemini CLI provides powerful commands to manage not just your conversation history, but also the state of your project files, giving you a safety net for AI-driven modifications.

### Managing Chat Sessions (`/chat` commands)

Instead of saving conversations to external files, the modern Gemini CLI uses a tag-based system to save and restore chat sessions directly within its internal history. This is extremely useful for creating checkpoints in a complex task.

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

## Context Management

Effective context management is key to collaborating efficiently with the AI. The following commands allow you to precisely view and control the context information being provided to the model.

*   **/show:** This is a very useful debugging command. It displays the full context that will be sent to the model on your **next** prompt submission. This includes:
    *   System instructions (e.g., from `GEMINI.md`)
    *   Content of files referenced with the `@` symbol
    *   Previous conversation history
    ```
    > @package.json
    > /show
    ```
    After running `/show`, you will clearly see that the content of `package.json` has been loaded and is ready to be used as context for the next request. This helps you confirm whether the AI is "seeing" the information you want it to see.

*   **/reload:** Reloads the system instructions from the `GEMINI.md` file. This command is very useful when you have modified `GEMINI.md` during a session and want the changes to take effect immediately without restarting the entire session.
    ```
    > /reload
    ```

## Other Useful Commands

*   **/theme:** Allows you to quickly switch or customize the interface's color theme.
    ```
    > /theme dark
    > /theme light
    > /theme custom blue purple
    ```

*   **/feedback:** Provides a quick channel to send any issues or suggestions you encounter directly to the Gemini CLI development team.
    ```
    > /feedback I hope a future version will add xxx feature.
    ```

*   **/quit** or **/exit:** Exits the Gemini CLI interactive session. You can also use the shortcut `Ctrl+D`.

Mastering these slash commands will give you much greater control over Gemini CLI and significantly improve your workflow efficiency. It is recommended that you try them out in practice to experience the convenience they offer.
