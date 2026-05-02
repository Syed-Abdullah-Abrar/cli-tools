# The Ultimate Stack: Master Architecture Reference

*Last Updated:* 2026-05-02

This is the **Master Index** for your entire CLI-AI Arsenal. Every tool, every script, every alias, and every config file in your system is documented across the files in this directory. If you forget how something works, start here.

---

## System Architecture

```
┌─────────────────────────── THE TERMINAL ───────────────────────────┐
│                                                                     │
│  ┌─ LAYER 1: TERMINAL FOUNDATION ──────────────────────────────┐   │
│  │  starship (prompt) · atuin (history) · tmux (sessions)      │   │
│  │  zoxide (navigation) · fzf (fuzzy everything)               │   │
│  │  eza · bat · ripgrep · fd (modern coreutils)                │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│  ┌─ LAYER 2: KNOWLEDGE & TASK SYSTEM ──────────────────────────┐   │
│  │  Taskwarrior (tasks) ←→ Tech-Goblet (notes) ←→ Git (sync)   │   │
│  │       ↑                        ↑                              │   │
│  │       │ Python hook            │ tg-capture.sh                │   │
│  │       ↓                        ↓                              │   │
│  │  Watson (time tracking)    llm embed-multi (semantic index)   │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│  ┌─ LAYER 3: AI ENGINE ───────────────────────────────────────┐    │
│  │  ai-chat.py (MiniMax-Text-01 bridge, "Arsenal Core")       │   │
│  │  Gemini CLI (interactive agentic agent)                      │   │
│  │  fabric (300+ structured prompt patterns)                    │   │
│  │  llm (model-agnostic Swiss knife + SQLite logging)           │   │
│  │  Ollama (local models — fully offline, zero API cost)        │   │
│  │  Aider (AI pair-programmer, Git-native)                      │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│  ┌─ LAYER 4: SPECIALIZED PROCESSORS ──────────────────────────┐   │
│  │  pdftotext + fabric (research paper analysis)                │   │
│  │  process-lecture (transcription → notes → quiz → Anki)       │   │
│  │  process-paper (PDF → summary → Tech-Goblet)                 │   │
│  │  project-status (AI meeting prep from tasks + time + notes)  │   │
│  │  smart-add (natural language → Taskwarrior command)           │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│  ┌─ LAYER 5: AUTOMATION & DOTFILE SYNC ───────────────────────┐   │
│  │  morning-briefing.sh (AI daily coach → Tech-Goblet)          │   │
│  │  evening-review.sh (AI daily review → Tech-Goblet)           │   │
│  │  rebuild-index.sh (semantic search reindexing)               │   │
│  │  gmobs-ingest.sh (Tech-Goblet AI synthesis)                  │   │
│  │  GNU Stow + GitHub (dotfile sync across machines)            │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## Documentation Index

| File | Covers |
| :--- | :--- |
| `_index.md` | This file. Master architecture diagram and navigation. |
| `taskwarrior.md` | Tasks, Watson hooks, contexts, dashboard. |
| `ai-engine.md` | MiniMax bridge, Gemini, fabric, semantic search, gmobs. |
| `starship.md` | Prompt layout, zero-latency Watson integration. |
| `tmux.md` | Session management, AI popup overlay. |
| `terminal-foundation.md` | atuin, zoxide, fzf, eza, bat, ripgrep, fd. |
| `neovim.md` | LazyVim, plugins, Gemini integration. |
| `workflows.md` | Daily rituals, lecture processing, paper analysis, project status. |
| `dotfiles.md` | GNU Stow architecture, GitHub backup, new machine restore. |

---

## Installation Status (as of 2026-05-02)

| Tool | Status | Version |
| :--- | :--- | :--- |
| starship | ✅ Installed | 1.24.2 |
| atuin | ✅ Installed | 18.13.6 |
| tmux | ✅ Installed | 3.4 |
| zoxide | ✅ Installed | 0.9.9 |
| fzf | ✅ Installed | 0.44.1 |
| eza | ✅ Installed | 0.23.4 |
| bat | ✅ Installed | 0.24.0 |
| ripgrep | ✅ Installed | 14.1.0 |
| fd | ✅ Installed | 9.0.0 |
| taskwarrior | ✅ Installed | 2.6.2 |
| watson | ✅ Installed | 2.1.0 |
| llm | ✅ Installed | 0.30 |
| git | ✅ Installed | 2.43.0 |
| gemini | ✅ Installed | Node-based |
| fabric | ✅ Installed | Go-based |
| ollama | ✅ Installed | 0.20.0 |
| aider | ✅ Installed | pip-based |
| pdftotext | ✅ Installed | system |
| faster-whisper | ❌ Not Installed | — |
| anki | ❌ Not Installed (GUI) | — |
