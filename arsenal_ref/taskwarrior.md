# Taskwarrior & Watson: The Time/Task Engine

## 1. What It Does
Taskwarrior is a high-performance command-line task manager. In our "Ultimate Stack", it is tightly integrated with Watson (a CLI time tracker). The goal is zero-friction task management: you add a task, you start working on it, and your computer automatically tracks your billable/study hours without you having to touch a second tool.

## 2. Where Things Are Located
- **Task Database**: `~/.task/` (Contains all your raw task data)
- **Task Config**: `~/.taskrc` (Your themes, contexts, and custom reports)
- **Watson Database**: `~/.config/watson/` (Contains all your time tracking data)
- **The Automagic Hook**: `~/.task/hooks/on-modify.py` (The bridge between Taskwarrior and Watson)

## 3. Core Aliases (How to use it)
These are mapped in `~/.dotfiles/shell/aliases.sh`:
- `t` (or `task`): View all pending tasks.
- `ta <description>`: Add a new task.
- `td` (or `task dash`): View the **Custom Dashboard** (Urgent tasks only).
- `tn` (or `task next`): View the next most important task.
- `tt <project>`: View tasks for a specific project.

### Basic Workflow
1. **Add**: `ta project:Personal priority:H "Fix neovim setup"`
2. **Start**: `task 1 start` *(This automatically starts Watson in the background!)*
3. **Stop**: `task 1 stop` *(This automatically stops Watson)*
4. **Complete**: `task 1 done` *(This completes the task and stops Watson)*

## 4. How the "Automagic Hook" Works
When you type `task 1 start`, Taskwarrior runs the python script located at `~/.task/hooks/on-modify.py`. 
The script reads the JSON data of the task. If it detects that the `start` parameter was just activated, it reads the `project:` tag of the task and silently executes `watson start <project>` in the background. If you mark the task as `done`, the script detects the status change and executes `watson stop`.

## 5. Contexts (Focus Mode)
If your task list is too cluttered, you can activate a **Context**. A context is a hard-filter that hides all irrelevant tasks until you turn it off.

- **Enable Study Mode**: `task context study` (Hides everything except `cs`, `math`, and `college` projects)
- **Enable Dev Mode**: `task context dev` (Hides personal and math projects)
- **Enable Focus Mode**: `task context focus` (Hides everything except tasks you have currently `started`)
- **Turn off Contexts**: `task context none`

## 6. How to Configure It
If you want to tweak colors, columns, or add new contexts, you edit the `~/.taskrc` file.
```bash
nvim ~/.taskrc
```

**Example: Adding a new Context**
To add a new context just for reading, append this to `~/.taskrc`:
`context.reading=+reading or project:books`

**Example: Tweaking the Dashboard**
Inside `~/.taskrc`, look for `report.dash.columns`. You can add or remove columns here. 
Available columns include: `id`, `project`, `priority`, `due`, `start.active`, `tags`, `description`, `urgency`.

## 7. Connection to the AI Arsenal
Every morning, your `morning-briefing.sh` script executes `task status:pending export`. It dumps your entire Taskwarrior database into a JSON blob, sends it to the MiniMax API via `ai-chat.py`, and asks the AI to recommend your #1 priority for the day. That recommendation is then logged into your Obsidian Tech-Goblet vault.

## 8. MCP Server Integration (Gemini CLI)
To allow AI agents (like the Gemini CLI) to natively read and modify your task list without raw bash execution, we use the Model Context Protocol (MCP).
- **Package**: `mcp-server-taskwarrior`
- **Fixer Script**: `~/scripts/taskwarrior-mcp-fix.js`
- **Why a fixer script?**: The raw MCP package has a missing schema declaration (`"type": "object"`) that causes strict clients to reject its tools. The fixer script runs the server and dynamically injects the missing schema, ensuring 100% compatibility with the Gemini CLI.
