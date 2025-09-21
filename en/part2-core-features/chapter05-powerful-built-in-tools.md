# Chapter 5: Powerful Built-in Tools

The power of Gemini CLI lies not only in its ability to understand and generate language, but also in its capacity to perform concrete "actions." These actions are provided by a series of built-in "Tools," which enable Gemini CLI to interact with your local environment and the internet to accomplish more complex tasks.

The model intelligently decides whether and how to use these tools based on your instructions. In this chapter, we will explore some of the most essential built-in tools.

## File System Tools

File system tools give Gemini CLI the ability to read and write local files directly. This means the AI can view file content, create new files, or modify existing ones, just like you can.

In previous chapters, when we asked the AI to generate `server.js` and save it, it was actually calling the `write-file` tool in the background.

### Common File Tools

*   `read_file`: Reads the content of a specified file.
*   `write_file`: Writes content to a specified file (overwriting it if it already exists).
*   `list_directory`: Lists the files and subdirectories within a specified directory.

### Practical Example: Code Refactoring

Imagine you have a very long function that you want to split into multiple files.

**Your Prompt:**
```
> I think the @src/utils.js file is too large.
> Please extract the `networkRequest` and `dataProcessing` functions from it,
> and move them into new files at @src/network.js and @src/data.js respectively.
> Then, delete these two functions from the original file (`utils.js`).
```

**Gemini CLI's Execution Process:**

1.  **Intelligent Planning:** Upon receiving the instruction, the model analyzes it as a multi-step file operation task.
2.  **Call `read_file`:** First, it calls the `read_file` tool to read the full content of `src/utils.js` to ensure it understands the functions to be moved.
3.  **Call `write_file`:** Next, it calls the `write_file` tool twice:
    *   First, to create `src/network.js` and write the `networkRequest` function's code into it.
    *   Second, to create `src/data.js` and write the `dataProcessing` function's code into it.
4.  **Call `write_file` again:** Finally, it calls `write_file` once more to update `src/utils.js`, removing the two functions that were extracted.
5.  **User Confirmation:** Before each critical file-writing operation, Gemini CLI will ask for your confirmation, ensuring that everything remains under your control.

In this way, Gemini CLI breaks down a complex refactoring task into a series of atomic calls to file system tools, automating the process while ensuring safety.

## Shell Tool

The Shell tool is an incredibly powerful feature that allows Gemini CLI to execute shell commands directly in your terminal. This greatly expands its capabilities, enabling it to perform a wide range of system-level tasks such as installing dependencies, running tests, and managing Git.

You can manually trigger the Shell tool by adding a `!` at the beginning of your prompt line.

### Practical Example: Automating Workflows

**Example 1: Installing dependencies and starting the project**
```
> !npm install && npm run dev
```
This command will first install the project's dependencies and then start the development server.

**Example 2: Creating a new branch and committing code**
```
> My code changes are complete.
> !git checkout -b feature/new-login-page
> !git add .
> !git commit -m "feat: implement new login page"
```

**Model-Automated Shell Tool Calls**

Even more powerfully, the model can automatically generate and execute shell commands based on your natural language instructions.

**Your Prompt:**
```
> Please check the code style of all TypeScript files in the project for me.
```

**Gemini CLI's Response:**
```
Of course, I will use the `npx eslint . --ext .ts` command to check the code style of the TypeScript files.

[TOOL] Executing: `npx eslint . --ext .ts`
[?] Continue? (Y/n)
```
The model identifies your intent and automatically generates the correct shell command. Before execution, it clearly displays the command that will be run and asks for your approval.

## Web Fetching Tools

Modern development is inseparable from accessing vast amounts of online information. Gemini CLI has built-in web fetching tools, giving it the ability to access the internet in real-time.

*   `web_fetch`: Fetches content from one or more URLs.
*   `google_web_search`: Performs a Google search and returns the results.

### Practical Examples

**Example 1: Learning a new technology**
```
> I want to learn how to use Zustand for state management in React.
> Please search for relevant official documentation or good tutorials and provide me with a beginner's example.
```
The model will call the `google_web_search` tool to search for "React Zustand tutorial" and may find the official Zustand documentation on GitHub. It will then call the `web_fetch` tool to read the content of the documentation page and, based on this information, summarize the core concepts and generate a starter code example for you.

**Example 2: Resolving an error message**
```
> My Next.js application is throwing an error on startup:
> "Error: connect ECONNREFUSED 127.0.0.1:6379"
> What usually causes this? Please search for a solution for me.
```
The model will use the `google_web_search` tool to search for this specific error message. Based on the search results (likely from Stack Overflow or GitHub Issues), it will analyze that this is typically caused by a local Redis service not running and will suggest the command to start the Redis service.

By combining its language capabilities with these powerful built-in tools, Gemini CLI transforms from a simple "chatbot" into an "intelligent agent" that can perceive its environment, perform actions, and acquire new knowledge, truly becoming a powerful assistant in your development workflow.

## Using Gemini CLI in Scripts and Automation (Headless Mode)

Beyond its interactive shell, Gemini CLI is also a powerful tool for automation. By using **headless mode**, you can integrate its AI capabilities directly into your scripts, CI/CD pipelines, and other automated workflows.

### How It Works
Headless mode runs the CLI non-interactively. Instead of a chat interface, it accepts a prompt directly, processes it, and prints the result to standard output before exiting. This makes it perfect for programmatic use.

You can provide input in two main ways:
1.  **Via the `--prompt` (or `-p`) flag:**
    ```bash
    gemini --prompt "Write a concise git commit message for the latest changes" --output-format json
    ```
2.  **Via standard input (stdin):** This allows you to pipe content from other commands directly into Gemini CLI.
    ```bash
    git diff --cached | gemini -p "Write a conventional commit message for these changes"
    ```

### Getting Structured Output
For automation, receiving a predictable, structured output is crucial. You can use the `--output-format json` flag to get a detailed JSON object as a response. This object includes not only the AI's textual response but also valuable metadata and statistics about model and tool usage.

```bash
# Get the response as JSON and use `jq` to extract just the text
gemini -p "What is the capital of France?" --output-format json | jq '.response'
```

> **Note on Compatibility:** The `--output-format json` flag is a powerful feature for scripting, but as of late 2025, it may not be available in all stable public releases of `gemini-cli` and could result in an "Unknown arguments" error. This feature is available in preview versions and is expected to be part of the standard command set. If you encounter this error, please check the official `gemini-cli` documentation for the latest command updates.

This combination of headless mode and structured JSON output unlocks endless possibilities for building AI-powered automations right from your command line.
