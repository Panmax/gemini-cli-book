# Chapter 2: Installation and Configuration

In this chapter, we will guide you through the installation and basic configuration process for Gemini CLI. The process is very straightforward, and in just a few steps, you can begin your AI coding journey.

## Environment Requirements

Before installing Gemini CLI, please ensure your development environment meets the following basic requirements:

*   **Node.js:** You need to have **Node.js v18 or higher** installed. Gemini CLI is built on Node.js, so this is the only strict requirement. You can check your current Node.js version by running the following command in your terminal:
    ```bash
    node -v
    ```
    If your version is below v18, or if you haven't installed Node.js yet, please visit the [official Node.js website](https://nodejs.org/) to download and install the latest version. We recommend using `nvm` (Node Version Manager) to manage multiple Node.js versions, which allows you to easily switch between different projects.

*   **Operating System:** Gemini CLI is cross-platform compatible and supports macOS, Windows, and Linux.

## Installation Steps

You can choose between two main installation methods based on your preference: global installation or quick execution via `npx`.

### Method 1: Global Installation (Recommended)

A global installation registers the `gemini` command in your system's path, allowing you to conveniently launch it from any location. This is the most recommended method.

Open your terminal and run the following command:

```bash
npm install -g @google/gemini-cli
```

`npm` is the package manager for Node.js and is usually installed along with it. The `-g` flag tells `npm` to install this package globally.

After the installation is complete, you can verify its success by running:

```bash
gemini --version
```

If the terminal outputs a version number, congratulations, Gemini CLI has been successfully installed!

### Method 2: Installation via Homebrew (macOS)

If you are a macOS user and have [Homebrew](https://brew.sh/) installed, you can use `brew` for the installation, which is often a more convenient option for macOS users.

```bash
brew install gemini-cli
```

Homebrew will automatically handle path and dependency management.

### Method 3: Quick Execution with `npx`

If you prefer not to perform a global installation, or if you just want to try it out temporarily, you can use `npx`. `npx` is a tool that allows you to run npm packages without first installing them.

In your terminal, run:

```bash
npx @google/gemini-cli
```

This will download the latest version of Gemini CLI and launch it immediately. Note that `npx` will re-download it each time, so for daily use, global installation is the more efficient choice.

## Authentication Methods

The first time you run Gemini CLI, it needs to obtain permission to access the Google Gemini API. Two authentication methods are currently supported.

### Google Account Login (Default)

When you run the `gemini` command for the first time, it will automatically open your default browser and guide you through the Google account login and authorization process.

1.  Type `gemini` in the terminal.
2.  A Google login page will automatically open in your browser.
3.  Select your Google account and log in.
4.  On the authorization page, click "Allow" to grant Gemini CLI the necessary API access permissions.
5.  Once completed, the browser will display a success page, and you can return to the terminal to start using it.

This method is very convenient and is recommended for beginners.

### API Key Configuration

For advanced users or in environments where browser login is not possible (e.g., CI/CD servers), you can authenticate by configuring an API key.

1.  Visit [Google AI Studio](https://aistudio.google.com/app/apikey) to obtain your API key.
2.  Once you have the key, set it in Gemini CLI using the following command:
    ```bash
    gemini config set GOOGLE_API_KEY "YOUR_API_KEY"
    ```
    Please replace `YOUR_API_KEY` with your own key.

## Understanding Configuration

Gemini CLI is highly configurable. You can control its behavior through command-line flags, environment variables, and `settings.json` files.

### Configuration Priority
Settings are applied in a specific order, with later sources overriding earlier ones:
1.  **`settings.json` Files (Lowest Priority):** Default settings are defined here.
2.  **Environment Variables:** System or session-specific variables (e.g., `GEMINI_MODEL`) can override settings from the files.
3.  **Command-line Flags (Highest Priority):** Flags used at launch (e.g., `--model gemini-1.5-pro`) will override all other settings for that session.

### The `settings.json` Configuration Files
Gemini CLI uses `settings.json` files for persistent configuration. There are two primary locations you will interact with:

- **User Settings File:** Located at `~/.gemini/settings.json`, this file holds your global settings that apply to all your projects.
- **Project Settings File:** You can create a `.gemini/settings.json` file inside your project's root directory. Settings in this file are specific to that project and will override your global user settings.

This layered approach allows you to set global preferences (like your favorite theme) while also defining project-specific rules (like the exact Gemini model to use).

A `settings.json` file contains many options, from UI tweaks to advanced tool configurations. You can find the location of your user settings file by running:
```bash
gemini config path
```

**Advanced Tip:** You can reference environment variables directly within your `settings.json` file using the `$VAR_NAME` or `${VAR_NAME}` syntax. This is a powerful way to manage secrets without hardcoding them.
```json
{
  "security": {
    "auth": {
      "apiKey": "$MY_SECRET_API_KEY"
    }
  }
}
```

### Personalizing Theme Settings
A common configuration is customizing the theme. You can set this in your user `settings.json` file. Gemini CLI comes with several built-in themes you can choose from.

**Available Built-in Themes:**
- **Dark Themes:** `ANSI`, `Atom One`, `Ayu`, `Default`, `Dracula`, `GitHub`
- **Light Themes:** `ANSI Light`, `Ayu Light`, `Default Light`, `GitHub Light`, `Google Code`, `Xcode`

To set a theme, for example `GitHub`, you would add the following to your `settings.json`:
```json
{
  "ui": {
    "theme": "GitHub"
  }
}
```
You can also define fully custom themes. For a full list of available settings and themes, refer to the official Gemini CLI documentation.
