# Neovim (LazyVim): The Code Editor

## 1. What It Does
Neovim is a hyper-extensible terminal-based text editor. You are running the **LazyVim** distribution, which is a pre-configured, batteries-included Neovim setup that comes with a curated set of plugins, keybindings, and a plugin manager called `lazy.nvim`.

## 2. Where Things Are Located
- **Config Directory (Real)**: `~/.dotfiles/neovim/.config/nvim/`
- **Config Directory (Symlink)**: `~/.config/nvim/` → points to the real directory above via GNU Stow
- **Plugin Manager Data**: `~/.local/share/nvim/lazy/` (auto-managed, do not manually edit)
- **Mason (LSP/Linter installs)**: `~/.local/share/nvim/mason/` (auto-managed)
- **Dotfiles Module**: Run `stow neovim` from `~/.dotfiles/` to link

## 3. Your Custom Plugins

### `lua/plugins/gemini.lua` — Gemini CLI Integration
Opens the Gemini AI agent directly inside a Neovim split.
- **Keybinding**: `<leader>og` (press Space, then `o`, then `g`)
- **Split direction**: Horizontal (configurable in the file)
- **What it does**: Toggles a terminal pane running `gemini` so you can have a live conversation with the AI while editing code

### `lua/plugins/formatting.lua` — Python Formatter
Configures `black` as the auto-formatter for Python files.
- **Line length**: 120 characters (wider than default 88)
- **Trigger**: Automatically formats on save via `conform.nvim`

### `lua/plugins/neorg.lua` — Neorg (Org-mode for Neovim)
A structured note-taking system inside Neovim (similar to Emacs Org-mode).
- **Status**: Loaded, but concealer icons are commented out
- **File type**: `.norg` files

### `lua/plugins/example.lua` — LazyVim Example Spec
This file is **disabled** (line 3: `if true then return {} end`). It is a reference template showing how to configure LazyVim plugins. You can safely ignore it, but it is a useful reference.

## 4. Built-in LazyVim Features (Pre-Installed)
LazyVim comes with dozens of plugins out of the box. Here are the ones you will use daily:

### Navigation
| Keybinding | Action |
| :--- | :--- |
| `<leader>ff` | **Find Files** (Telescope fuzzy finder) |
| `<leader>fg` | **Live Grep** (search file contents with ripgrep) |
| `<leader>fb` | **Find Buffers** (switch between open files) |
| `<leader>fr` | **Recent Files** (recently opened files) |
| `<leader>e` | **File Explorer** (Neo-tree sidebar) |

### Code Intelligence (LSP)
| Keybinding | Action |
| :--- | :--- |
| `gd` | **Go to Definition** (jump to where a function is defined) |
| `gr` | **Go to References** (find all usages of a symbol) |
| `K` | **Hover Documentation** (show docs for the symbol under cursor) |
| `<leader>ca` | **Code Action** (auto-fix suggestions) |
| `<leader>cr` | **Rename Symbol** (refactor across all files) |

### Window Management
| Keybinding | Action |
| :--- | :--- |
| `<leader>-` | Split window horizontally |
| `<leader>\|` | Split window vertically |
| `Ctrl+h/j/k/l` | Navigate between splits |
| `<leader>bd` | Close (delete) the current buffer |

### Git Integration
| Keybinding | Action |
| :--- | :--- |
| `<leader>gg` | Open **Lazygit** (full Git TUI inside Neovim) |
| `<leader>gf` | Git file history |
| `]c` / `[c` | Jump to next/previous Git change in a file |

## 5. Mason — Your LSP/Linter Manager
Mason automatically installs language servers, linters, and formatters. You have the following pre-configured:
- `stylua` — Lua formatter
- `shellcheck` — Shell script linter
- `shfmt` — Shell script formatter
- `flake8` — Python linter
- `black` — Python formatter (via conform.nvim)
- `pyright` — Python LSP (auto-installed)

To install additional tools, type `:Mason` inside Neovim and use the interactive UI.

## 6. How to Configure
To add a new plugin or change settings:
```bash
nvim ~/.config/nvim/lua/plugins/
```
Create a new `.lua` file (e.g., `my-plugin.lua`) and LazyVim will automatically pick it up on restart.

To update all plugins:
```
:Lazy update
```

## 7. Connection to the Arsenal
- **Gemini Integration**: `<leader>og` opens the AI agent inside Neovim
- **Tmux Overlay**: `Prefix + a` opens a floating terminal over Neovim for quick `ai` queries
- **Telescope + Tech-Goblet**: You can search your entire knowledge base from Neovim:
  ```
  <leader>ff → type ~/Tech-Goblet/wiki/ → fuzzy search your notes
  <leader>fg → grep across all your notes and code simultaneously
  ```
