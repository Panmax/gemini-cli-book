# Chapter 10: Project-Level Walkthroughs

Theoretical knowledge is important, but true mastery comes from practice. In this chapter, we will connect all the concepts we've learned—context management, tool usage, IDE integration, and more—through three complete project-level walkthroughs. These examples will show you how to use Gemini CLI to solve problems in real-world development scenarios.

For these exercises, we will assume we are working on a small open-source project called `todo-cli`, a Node.js tool for managing to-do lists from the command line.

---

## Case Study 1: Code Comprehension and Refactoring

**Scenario:** You have just taken over the `todo-cli` project and need to quickly familiarize yourself with its code structure and perform a simple refactoring of its core logic.

**Step 1: Clone the Project and Gain Initial Understanding**

First, let's use the shell tool to clone the project.

```
> !git clone https://github.com/example/todo-cli.git
> !cd todo-cli
```

The project is cloned, but we know nothing about the code. Now, let's leverage Gemini CLI's global comprehension capabilities.

```
> I want to quickly understand this project. Please analyze all the files in the @. directory
> and summarize the project's main features, technology stack, and code structure.
```

Gemini CLI will read all the files in the project (respecting `.gitignore` rules) and then provide a summary report, which might look like this:
> `todo-cli` is a command-line tool built with Node.js and the `yargs` library. Its main functions are to add, list, and complete to-do items. Data is stored in a local file named `tasks.json`. The core logic is located in `index.js`, which contains the implementation for all commands.

**Step 2: Focus on Core Logic and Propose Refactoring Suggestions**

From the summary, we know the core logic is in `index.js`. Now, let's dive deeper into this file and ask for refactoring advice.

```
> @index.js
> Please analyze this file. I think all the functionality is crammed together, and the code might be a bit messy.
> Do you have any good refactoring suggestions?
```

After analyzing `index.js`, Gemini CLI might respond:
> Yes, currently the implementations for all commands (add, list, done) are written directly in the `yargs` configuration. A good refactoring approach would be to extract the core business logic (like reading/writing to `tasks.json` and handling task data) into a separate module. For example, we could create a `lib/tasks.js` module to specifically manage tasks.

**Step 3: Apply the Code Changes**

That suggestion sounds good. Let's have Gemini CLI perform the refactoring.

```
> That's a great suggestion. Please go ahead and create a `lib/tasks.js` file,
> move all the logic related to reading and writing tasks (e.g., `loadTasks`, `saveTasks`, `addTask`, etc.)
> from `index.js` into the new file.
> Then, update `index.js` to call the functions from the new module.
```

Next, something magical happens. Gemini CLI will perform a series of operations in the background:
1.  **Read** the content of `index.js`.
2.  **Create** `lib/tasks.js` and write the extracted functions into it.
3.  **Rewrite** `index.js`, removing the old logic and adding `require('./lib/tasks')` along with calls to the new functions.

With [IDE integration](#chapter-8-integrated-development-environment-ide-integration) enabled, these changes will appear directly in your VS Code as a **diff view**, waiting for your review and approval. With just one click, a clean and safe code refactoring is complete.

---

## Case Study 2: Bug Fixing and Test Generation

**Scenario:** A user has reported a bug: when the `tasks.json` file does not exist, running the `todo-cli list` command causes the program to crash. We need to reproduce the issue, fix it, and add a unit test for the fix.

**Step 1: Locate and Fix the Bug**

We are not sure where the problem is, so let's give the error message directly to Gemini CLI.

```
> When the tasks.json file doesn't exist, the `list` command crashes with the error "ENOENT: no such file or directory".
> @index.js @lib/tasks.js
> Please help me locate the problem and generate a fix.
```

Gemini CLI will analyze the `loadTasks` function and quickly find the issue:
> The problem is in the `loadTasks` function in `lib/tasks.js`. It directly attempts to read the file using `fs.readFileSync` without handling the case where the file does not exist. You should check if the file exists before reading it, or use a `try...catch` block to handle the exception.

**Your next prompt:**
```
> Okay, please fix this using a try...catch block.
> In the catch block, if the error is "ENOENT", it should return an empty array `[]`, indicating there are no tasks.
```

Gemini CLI will immediately generate the corrected `loadTasks` function code and prompt you to apply the changes to the `lib/tasks.js` file.

**Step 2: Automatically Generate Unit Tests**

The bug is fixed, but to prevent similar issues in the future, we need to add unit tests.

```
> I need to write unit tests for the `lib/tasks.js` module using the `jest` framework.
> The tests need to cover the following scenarios:
> 1. When tasks.json exists and contains data, `loadTasks` reads it correctly.
> 2. When tasks.json does not exist, `loadTasks` returns an empty array (verifying our bug fix).
> 3. The `saveTasks` function is called correctly.
```

Gemini CLI will generate a complete test file for you, likely named `lib/tasks.test.js`, containing all the necessary `jest` code, mocks, and assertions. You just need to save the file and run the tests.

```
> !npx jest
```

---

## Case Study 3: Creating a New Feature from Scratch

**Scenario:** We want to add a brand new "archive" feature to `todo-cli`: allowing users to move completed tasks from the main list to an `archive.json` file.

**Step 1: Plan the New Feature**

First, let's plan the implementation steps by having a conversation with Gemini CLI.

```
> I want to add an `archive` command. Its functionality should be:
> 1. Read the `tasks.json` file.
> 2. Filter out all tasks with a status of "done".
> 3. Remove these completed tasks from `tasks.json`.
> 4. Append them to an `archive.json` file.
> Please outline the code modification steps to implement this feature.
```

Gemini CLI will provide a clear plan:
> Okay, here is a viable plan:
> 1.  In `lib/tasks.js`, add a new `archiveTasks` function to handle the core logic.
> 2.  This function will need to read `tasks.json` and `archive.json` (if it exists).
> 3.  It will filter the tasks and call `saveTasks` to update both files.
> 4.  In `index.js`, add a new command definition for `archive` that calls the `archiveTasks` function from `lib/tasks.js`.

**Step 2: Generate Code Step-by-Step**

With a clear plan, we can have Gemini CLI write the code for us, one step at a time.

```
> Okay, let's start with the first step. Please create the skeleton for the `archiveTasks` function in `lib/tasks.js`.
```
... apply code ...
```
> Great, now fill in the logic for filtering and file I/O in this function.
```
... apply code ...
```
> `lib/tasks.js` looks complete. Now please modify `index.js` to add the new `archive` command.
```
... apply code ...

**Step 3: Use Multimodal Capabilities to Generate Documentation Illustrations**

The new feature is complete, and we need to update the documentation. Maybe we can add a flowchart to it?

```
> I want to write documentation for the `archive` feature and need a flowchart to illustrate how it works.
> The flow includes: Start -> Read Tasks -> Filter Completed Tasks -> Update tasks.json -> Update archive.json -> End.
> Please generate a Graphviz (DOT language) description for this flowchart.
```

Gemini CLI will generate the DOT language code for you. You can paste this into a Graphviz tool to instantly create a clean flowchart, perfect for your project documentation.

Through these three case studies, you can see how Gemini CLI acts as an all-in-one development partner, deeply involved in the entire software development lifecycle—from initial code comprehension to refactoring, debugging, testing, and the creation of brand-new features.
