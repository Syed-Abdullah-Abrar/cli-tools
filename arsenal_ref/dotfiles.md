# Dotfiles: GNU Stow & GitHub Backup System

## 1. What It Does
Your entire terminal configuration is version-controlled in a single Git repository at `~/.dotfiles/`. The program **GNU Stow** creates symlinks (ghost files) from this repository out to the locations where the OS expects to find them. This means you can push your entire terminal setup to GitHub and restore it on any new machine in under 5 minutes.

## 2. Where Things Are Located
- **The Repository**: `~/.dotfiles/` (this is the Git repo)
- **GitHub Remote**: Run `cd ~/.dotfiles && git remote -v` to see your remote URL
- **Stow Binary**: `/usr/bin/stow` (installed via `sudo apt install stow`)

## 3. How GNU Stow Works (The Mental Model)

### The Problem
Your config files are scattered across your home directory:
```
~/.bashrc
~/.bash_aliases
~/.tmux.conf
~/.taskrc
~/.config/nvim/
~/.config/starship.toml
```
Git cannot track files in different locations. It can only track files inside a single folder.

### The Solution
We move all the real files into `~/.dotfiles/`, organized by "module":
```
~/.dotfiles/
├── bash/
│   ├── .bashrc         ← The REAL file
│   └── .bash_aliases   ← The REAL file
├── tmux/
│   └── .tmux.conf      ← The REAL file
├── taskwarrior/
│   ├── .taskrc          ← The REAL file
│   └── .task/hooks/on-modify.py
├── neovim/
│   └── .config/nvim/    ← The REAL directory
├── starship/
│   └── .config/starship.toml
├── scripts/
│   └── scripts/         ← Maps to ~/scripts/
└── ai-agents/
    └── .agents/workflows/
```

Then we run `stow bash` from inside `~/.dotfiles/`. Stow reads the directory structure inside `bash/` and creates symlinks in your home directory that point back:
```
~/.bashrc → .dotfiles/bash/.bashrc
~/.bash_aliases → .dotfiles/bash/.bash_aliases
```

### The Result
When the OS reads `~/.bashrc`, it silently follows the symlink to `~/.dotfiles/bash/.bashrc`. You edit files in their normal locations, and the changes are automatically reflected inside the Git repository.

## 4. Complete Module Map

| Stow Module | Contents | Symlink Target |
| :--- | :--- | :--- |
| `bash` | `.bashrc`, `.bash_aliases` | `~/` |
| `tmux` | `.tmux.conf` | `~/` |
| `vim` | `.vimrc` | `~/` |
| `neovim` | `.config/nvim/` (LazyVim) | `~/.config/` |
| `starship` | `.config/starship.toml` | `~/.config/` |
| `taskwarrior` | `.taskrc`, `.task/hooks/` | `~/` |
| `ai-agents` | `.agents/workflows/` | `~/` |
| `scripts` | `scripts/` (all custom scripts) | `~/` |

## 5. Daily Usage

### Making Changes
Just edit files in their normal locations:
```bash
nvim ~/.bashrc          # Edit your shell config
nvim ~/.taskrc          # Edit your Taskwarrior dashboard
nvim ~/.tmux.conf       # Edit your Tmux layout
```
The symlinks mean these edits are automatically reflected in `~/.dotfiles/`.

### Pushing to GitHub
```bash
cd ~/.dotfiles
git add .
git commit -m "describe your change"
git push origin main
```

### Checking What Changed
```bash
cd ~/.dotfiles
git status              # See modified files
git diff                # See exact changes
git log --oneline -10   # See recent commits
```

## 6. New Machine Restoration (Disaster Recovery)

### Step 1: Install Prerequisites
```bash
sudo apt update
sudo apt install git stow tmux neovim taskwarrior python3 python3-pip
```

### Step 2: Clone Your Brain
```bash
cd ~
git clone <your-github-url> ~/.dotfiles
git clone <your-tech-goblet-url> ~/Tech-Goblet
```

### Step 3: Deploy All Symlinks
```bash
cd ~/.dotfiles
stow bash tmux vim neovim starship taskwarrior ai-agents scripts
```

### Step 4: Install Python/Go Tools
```bash
pip install td-watson llm fabric aider-chat
```

### Step 5: Set API Keys
```bash
# Add to ~/.bashrc or set in your environment
export OPENAI_API_KEY="your_minimax_key"
```

### Step 6: Rebuild Semantic Index
```bash
source ~/.bashrc
reindex
```

Your entire terminal environment is now fully restored.

## 7. Common Stow Operations

| Command | What It Does |
| :--- | :--- |
| `cd ~/.dotfiles && stow bash` | Link the bash module |
| `cd ~/.dotfiles && stow -D bash` | **Unlink** the bash module (removes symlinks) |
| `cd ~/.dotfiles && stow -R bash` | **Restow** (unlink + relink, useful after restructuring) |
| `cd ~/.dotfiles && stow */` | Link ALL modules at once |

## 8. Adding a New Tool to Dotfiles
If you install a new tool (e.g., `alacritty`) and want to back up its config:

```bash
# 1. Create the module directory mirroring the target structure
mkdir -p ~/.dotfiles/alacritty/.config/alacritty/

# 2. Move the real config into dotfiles
mv ~/.config/alacritty/alacritty.yml ~/.dotfiles/alacritty/.config/alacritty/

# 3. Create the symlink
cd ~/.dotfiles && stow alacritty

# 4. Verify
ls -la ~/.config/alacritty/alacritty.yml
# Should show: alacritty.yml → ../../.dotfiles/alacritty/.config/alacritty/alacritty.yml

# 5. Commit
cd ~/.dotfiles && git add alacritty/ && git commit -m "feat: Track alacritty config"
```
