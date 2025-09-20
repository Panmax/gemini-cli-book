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

## Session Management

Session management commands allow you to save and resume entire conversations, which is extremely useful for handling complex problems that require long, multi-step solutions.

*   **/save [filename]:** Saves the complete history of the current session to a JSON file. If you don't provide a filename, one will be generated automatically.
    ```
    > /save my_debug_session
    ```
    After execution, a `my_debug_session.json` file will be created, containing your entire conversation with the AI.

*   **/resume <filename>:** Loads a session history from a previously saved JSON file. This will restore you to the state you were in when you saved, including all conversational context.
    ```
    > /resume my_debug_session.json
    ```
    This feature is ideal for:
    *   **Pausing and Resuming:** When you need to temporarily interrupt a complex task, you can `/save` first and then seamlessly pick up where you left off with `/resume`.
    *   **Templating Workflows:** You can create a template session with initial instructions for common tasks (like "code review" or "generate tests") and quickly start it with `/resume`.
    *   **Sharing and Collaboration:** You can share the saved session file with colleagues, allowing them to fully review your thought process and the steps you took to solve a problem.

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
