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

### Method 2: Quick Execution with `npx`

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

## The `settings.json` Configuration File Explained

Gemini CLI saves your settings in a file named `settings.json`. You can find the location of this file by running:

```bash
gemini config path
```

This JSON file contains your authentication information, preferences, and more. One common customization is the theme.

### Personalizing Theme Settings

Gemini CLI supports custom interface themes. You can change the theme colors using the `config set` command. For example, if you want to set the primary color to blue and the secondary color to purple, you can run:

```bash
gemini config set theme.primary "blue"
gemini config set theme.secondary "purple"
```

You can also directly edit the `settings.json` file for more complex color customizations, using hexadecimal color codes, for example:

```json
{
  "theme": {
    "primary": "#8A2BE2",
    "secondary": "#FF8C00",
    "danger": "#DC143C",
    "success": "#00FA9A"
  }
}
```

After completing these steps, your Gemini CLI is fully configured and ready to go. In the next chapter, we will officially begin our first interactive session.
