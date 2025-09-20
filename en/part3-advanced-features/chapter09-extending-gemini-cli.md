# Chapter 9: Extending Gemini CLI with Extensions

While Gemini CLI is powerful out of the box, its true potential is unlocked through a modern, shareable **extension system**. Extensions allow you to package prompts, custom commands, and even complex MCP servers into a user-friendly format that can be easily installed and shared.

This chapter focuses on this new, recommended way of extending Gemini CLI.

## Extension Management via the Command Line

A suite of extension management tools is available via the `gemini extensions` command. Note that these commands must be run from your system's shell, not from within the interactive Gemini CLI session.

### Installing an Extension
You can install an extension from a GitHub URL or a local directory. This is the primary way to add new capabilities to your CLI.
```bash
# Install from GitHub
gemini extensions install https://github.com/google-gemini/gemini-cli-security

# Install from a local directory
gemini extensions install --path=path/to/local/extension
```

### Other Management Commands
- **Uninstall:** `gemini extensions uninstall <extension-name>`
- **Disable/Enable:** `gemini extensions disable <extension-name>` (can also be used with `--scope=workspace` to disable only for the current project).
- **Update:** `gemini extensions update <extension-name>` or `gemini extensions update --all`.

## How Extensions Work

On startup, Gemini CLI looks for extensions in the `<home>/.gemini/extensions` directory. Each extension is a folder containing a `gemini-extension.json` file, which defines how the extension behaves.

### The `gemini-extension.json` File
This manifest file is the heart of the extension. Here is a sample structure:
```json
{
  "name": "my-extension",
  "version": "1.0.0",
  "mcpServers": {
    "my-server": {
      "command": "node my-server.js"
    }
  },
  "contextFileName": "GEMINI.md",
  "excludeTools": ["run_shell_command(rm -rf)"]
}
```
- **`name`**: The unique name of the extension.
- **`version`**: The extension's version.
- **`mcpServers`**: Defines and configures any MCP servers that the extension needs to run. This is the modern way to manage MCP servers, abstracting the complexity away from the user.
- **`contextFileName`**: A file within the extension that provides context to the model, similar to a project-level `GEMINI.md`.
- **`excludeTools`**: Allows the extension to disable certain tools for safety or relevance, e.g., blocking `rm -rf`.

### Custom Commands
Extensions can provide their own custom slash commands by placing `.toml` files in a `commands/` subdirectory. This is a powerful way to bundle reusable prompts and scripts.

If a custom command from an extension conflicts with a user's own command, the extension's command will be automatically renamed. For example, an extension's `/deploy` command might become `/my-extension.deploy` to avoid a conflict.

## Creating Your Own Extension

Gemini CLI makes it easy to start building your own extension.

### Creating a Boilerplate Extension
You can generate a new extension based on official examples:
```bash
gemini extensions new path/to/your/new/extension custom-commands
```
This command creates a new directory with a pre-configured `gemini-extension.json` and a sample command, giving you a perfect starting point.

### Linking for Development
To make development easier, you can use the `link` command. This creates a symbolic link from your development directory to the Gemini CLI extensions folder, so any changes you make are reflected immediately without needing to constantly update or reinstall.
```bash
gemini extensions link path/to/your/dev/extension
```
By leveraging this extension system, you can create powerful, reusable, and shareable tools that deeply integrate with your specific workflows.
