# Tmux: The Terminal Multiplexer

## 1. What It Does
Tmux lets you split your terminal into multiple panes and windows inside a single screen. If your SSH connection drops or your terminal crashes, your Tmux session survives and you can reattach instantly. It is the backbone of your entire coding environment.

## 2. Where Things Are Located
- **Config File (Real)**: `~/.dotfiles/tmux/.tmux.conf`
- **Config File (Symlink)**: `~/.tmux.conf` → points to the real file above via GNU Stow
- **Dotfiles Module**: `taskwarrior` (run `stow tmux` from `~/.dotfiles/`)

## 3. Current Configuration Breakdown

### Terminal Settings
| Setting | Value | Why |
| :--- | :--- | :--- |
| `default-terminal` | `screen-256color` | Enables 256-color support for Neovim and bat |
| `mouse` | `on` | Lets you click panes and scroll with your mouse |
| `history-limit` | `50000` | Stores 50k lines of scrollback per pane |
| `base-index` | `1` | Windows start at 1, not 0 (more intuitive) |
| `pane-base-index` | `1` | Panes also start at 1 |

### Keybindings
All keybindings use the **Prefix key** which is `Ctrl+b` (the default).

| Shortcut | Action |
| :--- | :--- |
| `Prefix + \|` | Split window **vertically** (side by side) |
| `Prefix + -` | Split window **horizontally** (top and bottom) |
| `Alt + Arrow Keys` | Navigate between panes without needing the Prefix |
| `Prefix + r` | Reload the tmux config and display "Config reloaded!" |
| `Prefix + a` | **🆕 AI Overlay**: Opens a floating bash popup (80% width, 70% height) directly over your current pane |

### Status Bar (The Bottom Strip)
| Element | Style | Description |
| :--- | :--- | :--- |
| Background | `#1a1b26` (Tokyo Night dark) | Matches your Neovim colorscheme |
| Left side | `#7aa2f7` bold | Shows the current **session name** |
| Right side | `#9ece6a` | Shows the current **time** |

## 4. The AI Overlay (`Prefix + a`)
This is a custom integration we built during the Arsenal deep-dive session.
When you press `Prefix + a`, Tmux spawns a **floating popup** using `display-popup`. This is a temporary bash shell that hovers directly over your code.

### Use Cases
- Type `ai "What does this Python error mean?"` to get instant help
- Type `n "Remember to refactor the auth module"` to capture a quick thought
- Type `seek "neural networks"` to semantic-search your knowledge base
- Press `Ctrl+d` to instantly close the overlay and return to your code

### Why a Popup and Not a Split?
A traditional split (`Prefix + |`) permanently rearranges your pane layout. The popup overlay is **ephemeral**—it floats on top without disturbing your carefully arranged coding environment.

## 5. Common Tmux Commands

| Command | What It Does |
| :--- | :--- |
| `tmux` | Start a new session |
| `tmux new -s work` | Start a named session called "work" |
| `tmux ls` | List all running sessions |
| `tmux a -t work` | Reattach to the "work" session |
| `tmux kill-session -t work` | Kill the "work" session |
| `Prefix + d` | Detach from current session (it keeps running) |
| `Prefix + c` | Create a new window |
| `Prefix + n` | Switch to the next window |
| `Prefix + p` | Switch to the previous window |
| `Prefix + [` | Enter scroll/copy mode (use `q` to exit) |

## 6. How to Configure
Edit the config file:
```bash
nvim ~/.tmux.conf
```
After saving, reload without restarting:
```bash
tmux source-file ~/.tmux.conf
```
Or from inside Tmux, press `Prefix + r`.
