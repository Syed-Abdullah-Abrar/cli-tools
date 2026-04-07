# 🤝 Contributing to CLI-AI Arsenal

First off, thank you for considering contributing to the **CLI-AI Arsenal**! 

This repository is designed to be the ultimate, lightweight, local-first blueprint for AI-augmented CLI productivity. Because the focus here is on **minimalism, zero vendor lock-in, and Unix-philosophy**, we are highly selective about what goes into the core stack.

## 🛠️ How You Can Contribute

We welcome contributions in the following areas:

### 1. New Agent Workflows (`.agents/workflows/`)
The true power of this Arsenal comes from the AI slash-commands. If you have built an incredibly efficient prompt pipeline (e.g., using `fabric`, `llm`, or `aider`) that processes text, writes code, or manages tasks natively in the terminal, we want it!
- Please submit your workflow as a new markdown file mimicking the existing `.md` workflow structures.
- Include step-by-step instructions.

### 2. Bash/Zsh Script Optimizations
If you can optimize the existing `morning-briefing.sh` or `evening-review.sh` to be faster, more POSIX-compliant, or more robust, open a Pull Request.

### 3. Setup Guide Improvements
If an upstream package (like `starship` or `Taskwarrior`) changes installation procedures and breaks our `install-guide.md`, please submit a quick PR updating the curl strings or apt instructions.

## 🚫 What We Don't Accept (Right Now)
To keep the Arsenal fast and indestructible, we generally do **not** accept:
- Heavy GUI apps or Electron wrappers.
- Cloud-dependent services that require monthly subscriptions (local-first models like Ollama are preferred).
- Complex shell frameworks (like bleeding-edge `oh-my-zsh` plugins) that bloat terminal startup time beyond ~50ms.

## 📝 Pull Request Process

1. **Fork the repo** and create your branch from `main` (`git checkout -b feature/amazing-new-workflow`).
2. **Test your code!** Before submitting a bash script or `.tmux.conf` edit, test it meticulously on your local machine to ensure it doesn't break basic terminal interactions.
3. **Draft the PR**. Describe exactly what your tool does, why it fits the CLI-AI philosophy, and how to verify its success.

## 🐛 Found a Bug?
If `stow` isn't symlinking correctly, or a regex sequence in one of the AI scripts is broken on a specific OS (like Arch vs WSL), please **open an issue** with:
- Your OS and Shell version (`uname -a`, `bash --version`).
- The exact error output from the terminal.

---

*Thank you for helping us build the ultimate terminal cockpit!*
