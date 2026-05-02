# 📦 CLI-AI Arsenal — Installation Log

> **Started:** 2026-04-06 14:32 UTC
> **System:** Linux (WSL/Ubuntu) · Bash · Python 3.12.3 · Node v25.8.1

---

## Pre-Install Audit

### Already Installed ✅
| Tool | Path | Notes |
|---|---|---|
| git | `/usr/bin/git` | — |
| curl | `/usr/bin/curl` | — |
| python3 | `/usr/bin/python3` | v3.12.3 |
| pip | `/usr/bin/pip` | — |
| node | `~/.nvm/versions/node/v25.8.1/bin/node` | v25.8.1 via nvm |
| npm | `~/.nvm/versions/node/v25.8.1/bin/npm` | — |
| tmux | `/usr/bin/tmux` | — |
| taskwarrior | `/usr/bin/task` | — |
| ollama | `/usr/local/bin/ollama` | — |

### Needs Installation ❌
- starship, zoxide, atuin, fzf, ripgrep, fd, bat, eza
- nb, watson
- fabric (needs Go first), llm, aider, faster-whisper
- chezmoi

---

## Phase 1: Terminal Foundation


## Phase 2: Knowledge & System Configuration
- Taskwarrior, tmux, git, curl, python, node are already present
- Installed Go and `fabric`
- Set up `starship.toml` and `.tmux.conf` configurations
- Integrated aliases into `.bash_aliases` for quick capture and automation workflows
- Configured automated cron jobs
- Created shell scripts `morning-briefing.sh`, `evening-review.sh`, and `rebuild-index.sh`
- Updated Gemini CLI `settings.json` with MCP capability for Taskwarrior and local search.

**Status: Setup is completed!** All configurations and automation are fully applied to the system.

## 2026-04-30: Tech-Goblet "Ultimate Stack" Migration
- **Deprecated `nb`**: The standalone `nb` utility was completely stripped from the setup.
- **Integrated `Tech-Goblet`**: The system now strictly uses the Hub-and-Spoke Obsidian vault (`~/Tech-Goblet/wiki`) as the unified brain.
- **Aliases Remapped**: Quick capture aliases (`n`, `ncs`, `nma`) now use `tg-capture.sh` to append directly into `Meta/Inbox.md` and `Readings/College_Notes.md`.
- **Scripts Migrated**: `morning-briefing.sh` and `evening-review.sh` now output logs to `Tech-Goblet/wiki/Daily/`.
- **Semantic Search**: `seek` now crawls the entire Obsidian vault.
