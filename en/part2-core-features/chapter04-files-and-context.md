# Chapter 4: Mastering Files and Context

In the first part, we learned the basics of Gemini CLI. Starting with this chapter, we will delve into its core features, the most crucial and powerful of which is its robust context management capability. Understanding and effectively using context is a necessary step on your path from novice to expert.

Context refers to the relevant information you provide to the Gemini model during your conversation. The more accurate and comprehensive the context, the more intelligent and tailored the model's responses will be to your actual needs. Gemini CLI offers several powerful mechanisms to help you easily build and manage context.

## Using `@` to Reference Files and Directories

This is one of the most frequently used and important features in Gemini CLI. By using the `@` symbol in your prompt, you can include the content of one or more files, or even an entire directory, as context for your query.

### Referencing a Single File

Suppose you have a file named `calculator.js` with the following content:
```javascript
// calculator.js
function add(a, b) {
  return a + b;
}
```
Now, you want to add a new subtraction function to this file. In Gemini CLI, you can do this:

```
> I'm working on the @calculator.js file. Please add a `subtract` function to it.
```

Gemini CLI will read the content of `calculator.js` and send it along with your prompt to the model. After understanding the existing code, the model will provide a complete version of the code that includes the new function. Since Gemini CLI knows which file you are modifying, it will also automatically prompt you to apply the changes.

### Referencing Multiple Files

When your feature's logic is spread across multiple files, you can reference them all at once.

```
> My API routes are defined in @routes/api.js, and the controller logic is in @controllers/userController.js.
> Please analyze these two files and explain the process flow for the `createUser` route.
```

### Referencing an Entire Directory

If you want Gemini CLI to have a comprehensive understanding of your entire project or a specific module, you can reference the directory directly.

```
> Please analyze all the files in the @src directory and generate a README.md file for this project.
```

Gemini CLI will recursively read all files in the `src` directory (while intelligently ignoring irrelevant files, which we will discuss later) and fulfill your request based on its understanding of the entire codebase.

## Handling Various File Types

Gemini CLI's multimodal capabilities allow it to handle more than just code files. You can include images, PDFs, and even audio and video files as part of your context.

*   **Code Review and UI Implementation:**
    ```
    > Here is the latest UI design for our product @design/homepage.png.
    > Please implement the basic layout of this page using React and Tailwind CSS.
    ```
*   **Document Understanding and Summarization:**
    ```
    > I need to quickly understand this open-source library. Here is its official documentation @docs/library.pdf.
    > Please summarize the core features and main APIs of this library for me.
    ```
*   **Error Analysis:**
    ```
    > My application crashed on a mobile device, and here is a screenshot of the crash @screenshots/error.jpg.
    > Can you identify the potential problem from this image?
    ```

## The Magic of the `GEMINI.md` File

`GEMINI.md` is a special file. When you create a `GEMINI.md` file in your project's root directory, Gemini CLI will automatically read its content at the beginning of each session and use it as a **global context** or **system instruction**.

This is ideal for defining project-level rules, code style guides, or providing background information.

A typical `GEMINI.md` file might look like this:

```markdown
# Gemini Guide: My Personal Blog Project

## Project Background
This is a personal blog built with Next.js and TypeScript. The code style follows the Airbnb JavaScript Style Guide.

## Coding Rules
- Prioritize functional components and Hooks.
- All components must have corresponding unit tests.
- API requests should use the `fetch` API with proper error handling.
- Commit messages must follow the Conventional Commits specification.

## My Role
You are a senior Web Development expert. Please follow all the rules above in your responses and provide suggestions in a professional and constructive tone.
```

With this file, Gemini CLI will "remember" these rules in every subsequent query. For example, when you ask it to create a new component, it will automatically follow your defined coding style and remind you to write tests.

## Using `.geminiignore` to Exclude Unnecessary Files

When you reference a directory, it often contains files you don't want the AI to see, such as `node_modules`, build artifacts (`dist`, `.next`), or files with sensitive information. To solve this, you can create a `.geminiignore` file in your project's root directory.

Its syntax largely follows the conventions of `.gitignore` files:
- Blank lines or lines starting with `#` are ignored.
- Standard glob patterns like `*` are supported.
- Ending a pattern with `/` ensures it only matches directories (e.g., `node_modules/`).
- Starting a pattern with `/` anchors it to the project root.
- Starting a pattern with `!` negates it, allowing you to re-include a file that was previously excluded.

A typical `.geminiignore` file:
```
# Ignore dependencies and build artifacts
/node_modules/
/dist/
/build/
.next/

# Ignore all log files
*.log

# But do not ignore this specific log file
!important.log

# Ignore environment variable files
.env
.env.local
```

**Important:** If you make changes to your `.geminiignore` file, you must **restart your Gemini CLI session** for the changes to be applied.

When reading a directory, Gemini CLI will automatically find and adhere to the rules in both `.geminiignore` and `.gitignore` files, ensuring that only the most relevant and secure content is provided as context to the model.

By mastering the use of `@` references, `GEMINI.md`, and `.geminiignore`, you can precisely control the context provided to the AI, thereby obtaining higher-quality and more relevant outputs, and truly unleashing the power of Gemini CLI.
