# Chapter 10: Security and Sandboxing

As a tool that can execute shell commands and modify files, Gemini CLI operates with a significant degree of power. While this is fantastic for productivity, it also introduces potential security risks, especially when the AI generates commands based on complex or ambiguous prompts.

To address this, Gemini CLI includes a robust **sandboxing** feature. This chapter will explain why sandboxing is crucial and how to use it to protect your system.

## Why is Sandboxing Important?

Sandboxing isolates potentially dangerous operations (like shell commands or file modifications) from your host system, creating a security barrier between the AI's actions and your environment. The key benefits include:

- **Security**: Prevents accidental system damage or data loss from unexpected commands (e.g., `rm -rf /`).
- **Isolation**: Limits file system access strictly to your current project directory.
- **Safety**: Reduces the risk of executing untrusted code or experimental commands suggested by the model.

## Sandboxing Methods

Gemini CLI offers different sandboxing methods depending on your operating system.

### 1. macOS Seatbelt (macOS only)
For macOS users, the CLI utilizes the lightweight, built-in `sandbox-exec` utility, also known as Seatbelt. It restricts operations like file writes to the project directory while still allowing network access by default. This provides a good balance of security and convenience with minimal setup.

### 2. Container-based (Docker/Podman)
For cross-platform sandboxing and maximum isolation, you can use a container-based approach with Docker or Podman. This method runs the CLI's tool execution environment inside a container, providing complete process and file system isolation. This is the most secure method but requires you to have Docker or Podman installed.

## How to Enable and Configure Sandboxing

You can enable sandboxing in three ways, listed in order of precedence:

**1. Command-line Flag (Highest Priority)**
The easiest way to enable sandboxing for a single session is with the `-s` or `--sandbox` flag.
```bash
gemini -s -p "run the test suite"
```

**2. Environment Variable**
You can enable it for your entire terminal session by setting an environment variable.
```bash
export GEMINI_SANDBOX=true
gemini -p "analyze the code structure"
```
You can also specify the method, e.g., `export GEMINI_SANDBOX=docker`.

**3. `settings.json` File (Default Setting)**
To enable sandboxing by default for all sessions, add the following to your `settings.json` file:
```json
{
  "tools": {
    "sandbox": true
  }
}
```
You can also specify the method here, e.g., `"sandbox": "docker"`.

## Troubleshooting Common Issues

- **"Operation not permitted"**: This is the sandbox working as intended! The AI tried to do something outside its allowed scope. If the action was intended, you may need to use a more permissive profile (for macOS) or adjust your container's volume mounts.
- **Missing commands inside the sandbox**: If a command works on your host but not in the sandbox, it's because the tool is not installed in the container. You may need to customize the Dockerfile to add the necessary dependencies.
- **Network issues**: If web-related tools fail, ensure your sandbox profile or container configuration allows network access.

By using the sandboxing feature, you can confidently leverage the full power of Gemini CLI's tools while maintaining a high level of security for your system.
