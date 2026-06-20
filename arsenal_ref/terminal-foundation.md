# Terminal Foundation: The Modern Coreutils Layer

## 1. Overview
These are the tools that replace the ancient, ugly default Unix utilities with blazing-fast, color-coded, human-friendly alternatives. They form the bedrock of your daily terminal navigation.

---

## 2. Atuin — Magical Shell History
**What it does**: Replaces your default `Ctrl+r` reverse search with a full-text, fuzzy, encrypted shell history database. It stores every command you have ever run and lets you search across sessions.

**Config Location**: `~/.config/atuin/config.toml`
**Data Location**: `~/.local/share/atuin/history.db`

### How to Use
| Action | Shortcut |
| :--- | :--- |
| Search history | `Ctrl+r` (type to fuzzy search) |
| Search from current dir only | `Ctrl+r` then toggle with `Ctrl+r` again |
| Stats about your usage | `atuin stats` |

### Key Features
- History is **encrypted** and can optionally sync to Atuin's cloud across machines
- Searches are fuzzy—typing `dock build` will match `docker build -t myapp .`
- Each entry records: command, directory, exit code, duration, and timestamp

---

## 3. Zoxide — Smarter `cd`
**What it does**: Learns your most-visited directories and lets you jump to them with a single keyword instead of typing full paths.

**Initialized in**: `~/.dotfiles/shell/aliases.sh` (sourced by both bash and zsh) via `eval "$(zoxide init bash)"` or `eval "$(zoxide init zsh)"`

### How to Use
| Command | What It Does |
| :--- | :--- |
| `z projects` | Jumps to the most frequently visited directory matching "projects" |
| `z pgmpy` | Jumps to `~/GSoC_projects/pgmpy_work/pgmpy` (if you visit it often) |
| `zi` | Interactive mode—fuzzy search all known directories |
| `zoxide query --list` | Show all directories zoxide has learned |

### How It Learns
Every time you `cd` into a directory, zoxide silently increments a "frecency" score (frequency + recency). The more you visit a path, the higher it ranks.

---

## 4. fzf — Fuzzy Finder for Everything
**What it does**: A general-purpose fuzzy finder that can filter any list of text. It powers `Ctrl+r` (via Atuin), file search, and can be piped into any command.

### How to Use
| Command | What It Does |
| :--- | :--- |
| `fzf` | Search all files in the current directory tree |
| `nvim $(fzf)` | Fuzzy-search for a file and open it in Neovim |
| `cat $(fzf)` | Fuzzy-search for a file and print its contents |
| `kill $(ps aux \| fzf \| awk '{print $2}')` | Fuzzy-search running processes and kill one |
| `Ctrl+t` | Paste a fuzzy-searched file path into your current command |
| `Alt+c` | Fuzzy-search directories and `cd` into one |

---

## 5. eza — Modern `ls`
**What it does**: Replaces the default `ls` with a colorful, icon-enabled, Git-aware file lister.

### Your Aliases (from `~/.dotfiles/shell/aliases.sh`)
| Alias | Expands To | What It Does |
| :--- | :--- | :--- |
| `ls` | `eza --icons` | List files with colorful icons |
| `ll` | `eza -l --icons` | Long listing with permissions, sizes, dates |
| `dir` | `eza --tree --git-ignore` | Show a tree view, respecting `.gitignore` |

---

## 6. bat — Modern `cat`
**What it does**: Replaces `cat` with syntax-highlighted, line-numbered, Git-diff-aware output.

### Your Alias
| Alias | Expands To |
| :--- | :--- |
| `bat` | `batcat` (Debian/Ubuntu names it `batcat` to avoid conflicts) |

### How to Use
| Command | What It Does |
| :--- | :--- |
| `bat file.py` | Print `file.py` with syntax highlighting and line numbers |
| `bat -A file.py` | Show all non-printable characters (tabs, newlines) |
| `bat --diff file.py` | Highlight Git changes within the file |
| `bat -l json data.txt` | Force JSON syntax highlighting on any file |

---

## 7. ripgrep (`rg`) — Blazing-Fast Search
**What it does**: Searches file contents at incredible speed. It respects `.gitignore`, skips binary files, and uses regex by default.

### How to Use
| Command | What It Does |
| :--- | :--- |
| `rg "def main"` | Search all files in the current tree for "def main" |
| `rg -i "todo"` | Case-insensitive search for "todo" |
| `rg "error" --type py` | Search only Python files |
| `rg "pattern" -l` | List only filenames that match (no content preview) |
| `rg "pattern" -C 3` | Show 3 lines of context around each match |
| `rg "pattern" ~/Tech-Goblet/wiki/` | Search your entire knowledge base |

---

## 8. fd — Modern `find`
**What it does**: Replaces the ancient `find` command with a fast, user-friendly alternative. Respects `.gitignore` by default.

### Your Alias
| Alias | Expands To |
| :--- | :--- |
| `fd` | `fdfind` (Debian/Ubuntu names it `fdfind`) |

### How to Use
| Command | What It Does |
| :--- | :--- |
| `fd "pattern"` | Find all files matching the pattern |
| `fd -e py` | Find all Python files |
| `fd -e md ~/Tech-Goblet/` | Find all Markdown files in your knowledge base |
| `fd -t d "src"` | Find only directories named "src" |
| `fd "test" -x rm` | Find and delete all files matching "test" (dangerous!) |
