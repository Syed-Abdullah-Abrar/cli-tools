# 🛠 CLI-AI Arsenal — Complete Setup Guide

> **Prerequisite:** Linux (Ubuntu/WSL). Bash or Zsh shell. `git`, `curl`, `python3`, `pip` installed.
> **Time to complete:** ~45 minutes for the full stack.
> **Strategy:** Install in layers. Each layer works independently. Don't skip Layer 1.

---

## Phase 1: Terminal Foundation (10 minutes)

> These tools make your terminal fast, intelligent, and pleasant to use. Install them first because everything else benefits from them.

### 1.1 Install Modern Coreutils

```bash
# ripgrep (faster grep)
sudo apt install ripgrep -y

# fd (faster find)
sudo apt install fd-find -y
# Note: on Ubuntu, binary is 'fdfind'. Create alias:
echo 'alias fd=fdfind' >> ~/.bashrc

# bat (better cat)
sudo apt install bat -y
# Note: on Ubuntu, binary is 'batcat'. Create alias:
echo 'alias bat=batcat' >> ~/.bashrc

# eza (better ls) — install via cargo or binary
cargo install eza 2>/dev/null || {
  sudo apt install gpg -y
  sudo mkdir -p /etc/apt/keyrings
  wget -qO- https://raw.githubusercontent.com/eza-community/eza/main/deb.asc | sudo gpg --dearmor -o /etc/apt/keyrings/gierens.gpg
  echo "deb [signed-by=/etc/apt/keyrings/gierens.gpg] http://deb.gierens.de stable main" | sudo tee /etc/apt/sources.list.d/gierens.list
  sudo apt update && sudo apt install eza -y
}

# fzf (fuzzy finder)
sudo apt install fzf -y

# tmux (session manager)
sudo apt install tmux -y

# FFmpeg (needed for transcription later)
sudo apt install ffmpeg -y

# poppler-utils (pdftotext — needed for research pipeline)
sudo apt install poppler-utils -y
```

### 1.2 Install Starship Prompt

```bash
curl -sS https://starship.rs/install.sh | sh

# Add to your shell config
echo 'eval "$(starship init bash)"' >> ~/.bashrc
# Or for Zsh:
# echo 'eval "$(starship init zsh)"' >> ~/.zshrc

# Create minimal config
mkdir -p ~/.config
cat > ~/.config/starship.toml << 'EOF'
# Minimal, fast prompt showing what matters
format = """
$directory\
$git_branch\
$git_status\
$python\
$nodejs\
$cmd_duration\
$line_break\
$character"""

[directory]
truncation_length = 3
truncation_symbol = "…/"

[git_branch]
symbol = " "

[cmd_duration]
min_time = 2_000
format = " [$duration]($style)"

[character]
success_symbol = "[❯](bold green)"
error_symbol = "[❯](bold red)"
EOF
```

### 1.3 Install Zoxide

```bash
curl -sSfL https://raw.githubusercontent.com/ajeetdsouza/zoxide/main/install.sh | sh

# Add to shell config
echo 'eval "$(zoxide init bash)"' >> ~/.bashrc
# Or for Zsh:
# echo 'eval "$(zoxide init zsh)"' >> ~/.zshrc
```

### 1.4 Install Atuin

```bash
curl --proto '=https' --tlsv1.2 -LsSf https://setup.atuin.sh | sh

# Add to shell config
echo 'eval "$(atuin init bash)"' >> ~/.bashrc
# Or for Zsh:
# echo 'eval "$(atuin init zsh)"' >> ~/.zshrc

# Optional: set up sync across machines
# atuin register -u yourusername -e youremail -p yourpassword
# atuin sync
```

### 1.5 Apply Changes

```bash
source ~/.bashrc  # or restart your terminal
```

### 1.6 Verify Layer 1

```bash
starship --version    # Should print version
zoxide --version      # Should print version
atuin --version       # Should print version
rg --version          # Should print version
fzf --version         # Should print version
tmux -V               # Should print version
```

---

## Phase 2: Knowledge & Task System (10 minutes)

### 2.1 Install Taskwarrior

```bash
sudo apt install taskwarrior -y

# Create your project structure
task add "Complete CLI-AI Arsenal setup" project:meta priority:H due:today

# Configure custom dashboard report — append to ~/.taskrc
cat >> ~/.taskrc << 'EOF'

# === CUSTOM REPORTS ===
report.dash.description=Daily Dashboard
report.dash.columns=id,project,priority,due.relative,description.truncated_count
report.dash.filter=status:pending -WAITING limit:15
report.dash.sort=urgency-

# === URGENCY TWEAKS ===
# Boost urgency for things due soon
urgency.due.coefficient=12.0
# Boost high-priority items more
urgency.priority.coefficient=6.0
# Projects you're actively working on get a boost
urgency.active.coefficient=4.0
EOF

# Add your initial projects
task add "Set up nb notebooks" project:meta priority:M due:today
task add "Configure fabric patterns" project:meta priority:M due:today
```

### 2.2 Install nb

```bash
# Install via npm (easiest on Ubuntu)
npm install -g nb.sh 2>/dev/null || {
  # Alternative: install from GitHub
  sudo curl -L https://raw.github.com/xwmx/nb/master/nb -o /usr/local/bin/nb
  sudo chmod +x /usr/local/bin/nb
  sudo nb completions install --download
}

# Create your notebook structure
nb notebooks add degree-cs
nb notebooks add degree-math
nb notebooks add pgmpy
nb notebooks add projects
nb notebooks add research
nb notebooks add daily-logs
nb notebooks add personal

# Create a daily log template
nb add daily-logs/ --title "TEMPLATE-daily-log" --content "# Daily Log — {{date}}

## 🎯 Focus Today


## 📝 Notes & Insights


## 🔑 Decisions Made


## 💡 Ideas


## 📌 Tomorrow
"

# Enable Git sync for all notebooks
nb remote set https://github.com/yourusername/nb-notes.git 2>/dev/null || true

# Quick test
nb add "First note: CLI-AI Arsenal is operational! 🚀"
nb search "Arsenal"
```

### 2.3 Install Watson (Time Tracking)

```bash
pip install td-watson

# Quick test
watson start meta "Setting up CLI tools"
# We'll stop this at the end of setup
```

### 2.4 Verify Layer 2

```bash
task dash                  # Should show your tasks
nb list                    # Should show your first note
watson status              # Should show running timer
```

---

## Phase 3: AI Engine (15 minutes)

### 3.1 Verify Gemini CLI (you already have this)

```bash
gemini --version  # Should work if already installed
```

### 3.2 Install fabric

```bash
# Requires Go. Install Go if needed:
which go || {
  sudo apt install golang-go -y
}

# Install fabric
go install github.com/danielmiessler/fabric@latest

# Add Go bin to PATH if not already
echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.bashrc
source ~/.bashrc

# Setup — enter your Google AI API key when prompted
fabric --setup

# Test it
echo "The quick brown fox jumps over the lazy dog" | fabric -p summarize
```

**Create custom patterns for your workflow:**

```bash
# Daily review pattern
mkdir -p ~/.config/fabric/patterns/daily_review
cat > ~/.config/fabric/patterns/daily_review/system.md << 'PATTERN'
# IDENTITY
You are an expert personal productivity coach analyzing a student's daily data.

# GOAL
Generate a concise daily review highlighting accomplishments, risks, and tomorrow's #1 priority.

# STEPS
- Analyze completed tasks and time logs
- Identify the most impactful accomplishment
- Flag any overdue or at-risk items
- Recommend the single most important action for tomorrow
- Keep total output under 200 words

# OUTPUT FORMAT
## ✅ Top Win
(one sentence)

## ⚠️ At Risk
(bullet list of slipping items, or "Nothing — on track!")

## 🎯 Tomorrow's #1 Priority
(one clear, specific action)

## 💡 Observation
(one insight about time allocation or patterns)
PATTERN

# Study note processor
mkdir -p ~/.config/fabric/patterns/study_notes
cat > ~/.config/fabric/patterns/study_notes/system.md << 'PATTERN'
# IDENTITY
You are an expert academic tutor processing lecture or reading content for a university student.

# GOAL
Transform raw academic content into structured, retention-optimized study notes.

# STEPS
- Identify the 3-5 core concepts
- For each concept: provide a clear definition, an intuition/analogy, and a concrete example
- Identify connections to prerequisite knowledge
- Generate 3 self-test questions at the end
- Flag any areas that seem incomplete or need clarification

# OUTPUT FORMAT
## Core Concepts

### 1. [Concept Name]
**Definition:** ...
**Intuition:** ...
**Example:** ...

### 2. [Concept Name]
(repeat)

## 🔗 Connections to Prior Knowledge
- ...

## ❓ Self-Test Questions
1. ...
2. ...
3. ...

## ⚠️ Needs Clarification
- ...
PATTERN
```

### 3.3 Install llm

```bash
pip install llm

# Install Gemini plugin (use your Google One AI Pro key)
llm install llm-gemini

# Set your API key
llm keys set gemini
# Paste your API key when prompted

# Set Gemini as default model (covered by your subscription)
llm models default gemini-2.5-flash

# Install Ollama plugin for local/offline models
llm install llm-ollama

# Test
echo "What is the capital of France?" | llm

# Verify logging works
llm logs list --count 1
```

### 3.4 Install Ollama (Local/Offline AI)

```bash
curl -fsSL https://ollama.ai/install.sh | sh

# Pull essential models (requires internet, one-time only)
# Small fast model for quick tasks
ollama pull llama3.2:3b

# Better quality model for serious work
ollama pull llama3.2:8b

# Embedding model for local semantic search
ollama pull nomic-embed-text

# Test offline capability
ollama run llama3.2:3b "Hello, are you working?"
# Type /bye to exit

# Verify llm can use Ollama
echo "test" | llm -m llama3.2:3b
```

### 3.5 Install Aider

```bash
pip install aider-chat

# Configure to use Gemini by default
echo 'export GEMINI_API_KEY=your-key-here' >> ~/.bashrc
source ~/.bashrc

# Test (in any Git repo)
cd ~/projects/some-repo 2>/dev/null || {
  mkdir -p /tmp/test-aider && cd /tmp/test-aider && git init
  echo "print('hello')" > test.py && git add . && git commit -m "init"
}
aider --model gemini/gemini-2.5-flash
# Type: "Add a docstring to the test.py file"
# Type: /exit
```

### 3.6 Build Semantic Search Index (THE WILDCARD)

```bash
# Build the initial semantic index over all your nb notes
find ~/.nb/ -name "*.md" | head -50 | xargs llm embed-multi notes -m 3-small --store

# Test semantic search
llm similar notes -c "productivity and task management" | head -3

echo "✅ Semantic search index built!"
```

### 3.7 Verify Layer 3

```bash
fabric --help | head -5        # Should show fabric help
llm --version                   # Should print version
ollama --version                # Should print version
aider --version                 # Should print version
llm similar notes -c "test"     # Should return results
```

---

## Phase 4: Specialized Processors (5 minutes)

### 4.1 Install faster-whisper

```bash
pip install faster-whisper

# Verify FFmpeg (installed in Phase 1)
ffmpeg -version | head -1

# Test with a short audio file if you have one:
# faster-whisper your-file.mp3 --model tiny --output_format txt
# The 'tiny' model downloads fast for testing; use 'large-v3' for production
```

### 4.2 Install Anki (optional — GUI app)

```bash
# Install Anki
sudo snap install anki-woodrow 2>/dev/null || {
  # Alternative: download from ankiweb.net
  echo "Download Anki from https://apps.ankiweb.net/ and install manually"
}

# Install AnkiConnect addon (for API access from CLI)
# Open Anki → Tools → Add-ons → Get Add-ons → Code: 2055492159
echo "After installing Anki, add AnkiConnect addon with code: 2055492159"
```

### 4.3 Install Zotero (optional — for citations)

```bash
# Download from https://www.zotero.org/download/
# Install Better BibTeX addon for LaTeX integration
echo "Download Zotero from https://www.zotero.org/download/"
echo "Install Better BibTeX addon for LaTeX citation export"
```

---

## Phase 5: Automation & Sync (5 minutes)

### 5.1 Create Automation Scripts

```bash
mkdir -p ~/scripts

# Morning Briefing Script
cat > ~/scripts/morning-briefing.sh << 'SCRIPT'
#!/bin/bash
set -e

BRIEFING=$(mktemp)
echo "# 🌅 Morning Briefing — $(date +%Y-%m-%d)" > "$BRIEFING"
echo "" >> "$BRIEFING"

echo "## 📋 Top Tasks (by urgency)" >> "$BRIEFING"
task rc.verbose=nothing dash 2>/dev/null >> "$BRIEFING" || echo "No pending tasks" >> "$BRIEFING"
echo "" >> "$BRIEFING"

echo "## ⏰ Overdue" >> "$BRIEFING"
task overdue 2>/dev/null >> "$BRIEFING" || echo "None! 🎉" >> "$BRIEFING"
echo "" >> "$BRIEFING"

echo "## 📊 Time This Week" >> "$BRIEFING"
watson report --week --no-pager 2>/dev/null >> "$BRIEFING" || echo "No time logged yet" >> "$BRIEFING"
echo "" >> "$BRIEFING"

# AI focus recommendation (skip if API unavailable)
echo "## 🤖 AI Recommendation" >> "$BRIEFING"
task export status:pending 2>/dev/null | llm "You are a productivity coach. Given these tasks, what is the ONE thing to do first today? Answer in 2 sentences." >> "$BRIEFING" 2>/dev/null || echo "AI recommendation unavailable" >> "$BRIEFING"

nb daily-logs:add --title "Briefing $(date +%Y-%m-%d)" --content "$(cat "$BRIEFING")" 2>/dev/null
cat "$BRIEFING"
rm "$BRIEFING"
SCRIPT
chmod +x ~/scripts/morning-briefing.sh

# Evening Review Script
cat > ~/scripts/evening-review.sh << 'SCRIPT'
#!/bin/bash
set -e

REVIEW=$(mktemp)
echo "# 🌙 Evening Review — $(date +%Y-%m-%d)" > "$REVIEW"
echo "" >> "$REVIEW"

echo "## ✅ Completed Today" >> "$REVIEW"
task end.after:today completed 2>/dev/null >> "$REVIEW" || echo "Nothing completed" >> "$REVIEW"
echo "" >> "$REVIEW"

echo "## ⏱ Time Today" >> "$REVIEW"
watson report --day --no-pager 2>/dev/null >> "$REVIEW" || echo "No time logged" >> "$REVIEW"
echo "" >> "$REVIEW"

echo "## 🤖 AI Reflection" >> "$REVIEW"
{
  task end.after:today completed 2>/dev/null
  echo "---"
  watson report --day --no-pager 2>/dev/null
} | fabric -p daily_review >> "$REVIEW" 2>/dev/null || echo "AI reflection unavailable" >> "$REVIEW"

nb daily-logs:add --title "Review $(date +%Y-%m-%d)" --content "$(cat "$REVIEW")" 2>/dev/null
cat "$REVIEW"
rm "$REVIEW"
SCRIPT
chmod +x ~/scripts/evening-review.sh

# Semantic Index Rebuild Script
cat > ~/scripts/rebuild-index.sh << 'SCRIPT'
#!/bin/bash
echo "🔄 Rebuilding semantic search indexes..."

# Notes index
find ~/.nb/ -name "*.md" 2>/dev/null | xargs llm embed-multi notes -m 3-small --store 2>/dev/null
echo "  ✅ Notes index rebuilt"

# Research index
if [ -d ~/research/summaries ]; then
  find ~/research/summaries/ -name "*.md" 2>/dev/null | xargs llm embed-multi research -m 3-small --store 2>/dev/null
  echo "  ✅ Research index rebuilt"
fi

echo "🏁 Done: $(date)"
SCRIPT
chmod +x ~/scripts/rebuild-index.sh
```

### 5.2 Schedule Automations (systemd timers or cron)

```bash
# Using cron (simpler)
(crontab -l 2>/dev/null; cat << 'CRON'
# Morning briefing at 7:00 AM
0 7 * * * /home/syed/scripts/morning-briefing.sh >> /tmp/briefing.log 2>&1
# Evening review at 9:00 PM
0 21 * * * /home/syed/scripts/evening-review.sh >> /tmp/review.log 2>&1
# Rebuild semantic index every Sunday at 2:00 AM
0 2 * * 0 /home/syed/scripts/rebuild-index.sh >> /tmp/index.log 2>&1
CRON
) | crontab -

echo "✅ Cron jobs scheduled"
crontab -l
```

### 5.3 Shell Aliases & Functions

Add these to your `~/.bashrc` (or `~/.zshrc`):

```bash
cat >> ~/.bashrc << 'ALIASES'

# ================================================================
# CLI-AI ARSENAL — Shell Integration
# ================================================================

# --- Quick capture ---
alias n="nb add"                                    # Quick note
alias nd="nb daily-logs:add"                        # Daily log entry
alias ncs="nb degree-cs:add"                        # CS note
alias nma="nb degree-math:add"                      # Math note
alias ns="nb search --all"                          # Search all notes

# --- Tasks ---
alias t="task"
alias ta="task add"
alias td="task dash"                                # Dashboard
alias tn="task next"                                # Next action
alias tt="task project:"                            # Start typing project filter

# --- Time tracking ---
alias ws="watson start"
alias wp="watson stop"
alias wr="watson report"
alias wrd="watson report --day"
alias wrw="watson report --week"

# --- AI shortcuts ---
alias ai="llm"                                      # Quick AI query
alias ailocal="llm -m llama3.2:8b"                  # Local/offline AI
alias wisdom="fabric -p extract_wisdom"             # Extract insights
alias quiz="fabric -p create_quiz"                  # Generate quiz
alias study="fabric -p study_notes"                 # Process study material
alias summ="fabric -p summarize"                    # Summarize

# --- Semantic search ---
alias seek="llm similar notes -c"                   # Semantic note search
alias seekr="llm similar research -c"               # Semantic research search

# --- Combined workflows ---
morning() { ~/scripts/morning-briefing.sh; }
evening() { ~/scripts/evening-review.sh; }
reindex() { ~/scripts/rebuild-index.sh; }

# Process a lecture recording into notes, quiz, and Anki cards
process-lecture() {
  local input="$1"
  local title="${2:-Lecture}"
  local notebook="${3:-degree-cs}"

  echo "🎤 Transcribing..."
  faster-whisper "$input" --model large-v3 --output_format txt > /tmp/lecture-transcript.txt

  echo "📝 Extracting wisdom..."
  cat /tmp/lecture-transcript.txt | fabric -p extract_wisdom > /tmp/lecture-wisdom.md
  nb "${notebook}:add" --title "${title} — Wisdom" --content "$(cat /tmp/lecture-wisdom.md)"

  echo "📝 Creating study notes..."
  cat /tmp/lecture-transcript.txt | fabric -p study_notes > /tmp/lecture-study.md
  nb "${notebook}:add" --title "${title} — Study Notes" --content "$(cat /tmp/lecture-study.md)"

  echo "❓ Generating quiz..."
  cat /tmp/lecture-transcript.txt | fabric -p create_quiz > /tmp/lecture-quiz.md
  nb "${notebook}:add" --title "${title} — Quiz" --content "$(cat /tmp/lecture-quiz.md)"

  echo "🃏 Generating Anki cards..."
  cat /tmp/lecture-quiz.md | llm "Convert these Q&A pairs into Anki TSV import format. One card per line: front<TAB>back. No headers or extra text." > /tmp/anki-cards.tsv

  echo ""
  echo "✅ Done! Created:"
  echo "   📒 Wisdom note in ${notebook}"
  echo "   📒 Study notes in ${notebook}"
  echo "   📒 Quiz in ${notebook}"
  echo "   🃏 Anki cards at /tmp/anki-cards.tsv"
  echo ""
  echo "Import Anki cards: Open Anki → File → Import → select /tmp/anki-cards.tsv"
}

# Summarize a PDF research paper
process-paper() {
  local pdf="$1"
  local title="${2:-$(basename "$pdf" .pdf)}"

  echo "📄 Processing: $title"
  pdftotext "$pdf" - | fabric -p summarize > "/tmp/${title}-summary.md"

  mkdir -p ~/research/summaries
  cp "/tmp/${title}-summary.md" ~/research/summaries/
  nb research:add --title "Summary: ${title}" --content "$(cat "/tmp/${title}-summary.md")"

  echo "✅ Summary saved to nb research notebook and ~/research/summaries/"
  cat "/tmp/${title}-summary.md"
}

# Quick project status for meetings
project-status() {
  local project="$1"
  {
    echo "TASKS:"
    task project:"$project" list 2>/dev/null
    echo ""
    echo "RECENT NOTES:"
    nb "${project}:list" --limit 5 2>/dev/null
    echo ""
    echo "TIME (this month):"
    watson report --month --project "$project" --no-pager 2>/dev/null
  } | llm "Generate a concise project status update for a 5-minute meeting. Include: progress, blockers, next steps, time invested. Be specific."
}

# Smart task creation with AI
smart-add() {
  local description="$*"
  echo "$description" | llm "Convert this into a Taskwarrior 'task add' command. Include appropriate project:, priority:, and due: if mentioned or implied. Output ONLY the command, nothing else." | bash
  echo "✅ Task added!"
  task next limit:3
}

ALIASES

source ~/.bashrc
```

### 5.4 Install chezmoi (Dotfile Management)

```bash
sh -c "$(curl -fsLS get.chezmoi.io)"

# Initialize and track your configs
chezmoi init

# Add your key config files
chezmoi add ~/.bashrc
chezmoi add ~/.taskrc
chezmoi add ~/.config/starship.toml
chezmoi add ~/.config/atuin/config.toml
chezmoi add ~/.tmux.conf 2>/dev/null

# Commit
chezmoi cd
git add . && git commit -m "Initial CLI-AI Arsenal config"
# Optionally push to GitHub:
# git remote add origin https://github.com/yourusername/dotfiles.git
# git push -u origin main
```

### 5.5 Install Syncthing (Optional — Multi-Device Sync)

```bash
sudo apt install syncthing -y

# Start Syncthing
syncthing &

# Access web UI at http://localhost:8384
# Add folders to sync:
# - ~/.task/     (Taskwarrior data)
# - ~/.nb/       (All notebooks)
# - ~/research/  (Papers and summaries)
echo "Configure Syncthing at http://localhost:8384"
echo "Share folders: ~/.task/, ~/.nb/, ~/research/"
```

---

## Phase 6: MCP Integration (3 minutes)

### 6.1 Configure MCP for Gemini CLI

```bash
# Ensure Node.js is installed (needed for MCP servers)
which node || {
  curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
  sudo apt install nodejs -y
}

# Create Gemini CLI MCP config
mkdir -p ~/.gemini
cat > ~/.gemini/settings.json << 'MCP'
{
  "mcpServers": {
    "taskwarrior": {
      "command": "npx",
      "args": ["-y", "mcp-server-taskwarrior"]
    },
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/home/syed/.nb",
        "/home/syed/projects",
        "/home/syed/research"
      ]
    }
  }
}
MCP

echo "✅ MCP configured for Gemini CLI"
echo "Try: gemini 'What are my pending tasks?'"
```

---

## Phase 7: Tmux Configuration (2 minutes)

```bash
cat > ~/.tmux.conf << 'TMUX'
# --- Basics ---
set -g default-terminal "screen-256color"
set -g mouse on
set -g history-limit 50000
set -g base-index 1
setw -g pane-base-index 1

# --- Keybindings ---
# Split panes with | and -
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"

# Easy pane switching with Alt+arrow
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

# Reload config
bind r source-file ~/.tmux.conf \; display "Config reloaded!"

# --- Status bar ---
set -g status-style 'bg=#1a1b26 fg=#a9b1d6'
set -g status-left '#[fg=#7aa2f7,bold] #S '
set -g status-right '#[fg=#9ece6a] %H:%M '
set -g status-left-length 30
TMUX

# Create default sessions for your contexts
tmux new-session -d -s main
tmux new-session -d -s cs
tmux new-session -d -s math
tmux new-session -d -s projects

echo "✅ Tmux configured with sessions: main, cs, math, projects"
echo "Attach with: tmux attach -t main"
```

---

## Phase 8: Final Verification

```bash
echo "========================================"
echo " CLI-AI ARSENAL — VERIFICATION"
echo "========================================"
echo ""

# Layer 1
echo "--- Layer 1: Terminal Foundation ---"
starship --version 2>/dev/null && echo "  ✅ starship" || echo "  ❌ starship"
zoxide --version 2>/dev/null && echo "  ✅ zoxide" || echo "  ❌ zoxide"
atuin --version 2>/dev/null && echo "  ✅ atuin" || echo "  ❌ atuin"
fzf --version 2>/dev/null && echo "  ✅ fzf" || echo "  ❌ fzf"
tmux -V 2>/dev/null && echo "  ✅ tmux" || echo "  ❌ tmux"
rg --version 2>/dev/null | head -1 && echo "  ✅ ripgrep" || echo "  ❌ ripgrep"

echo ""
echo "--- Layer 2: Knowledge & Tasks ---"
task --version 2>/dev/null && echo "  ✅ taskwarrior" || echo "  ❌ taskwarrior"
nb version 2>/dev/null && echo "  ✅ nb" || echo "  ❌ nb"
watson --version 2>/dev/null && echo "  ✅ watson" || echo "  ❌ watson"

echo ""
echo "--- Layer 3: AI Engine ---"
fabric --version 2>/dev/null && echo "  ✅ fabric" || echo "  ❌ fabric"
llm --version 2>/dev/null && echo "  ✅ llm" || echo "  ❌ llm"
ollama --version 2>/dev/null && echo "  ✅ ollama" || echo "  ❌ ollama"
aider --version 2>/dev/null && echo "  ✅ aider" || echo "  ❌ aider"

echo ""
echo "--- Layer 4: Processors ---"
python3 -c "import faster_whisper" 2>/dev/null && echo "  ✅ faster-whisper" || echo "  ❌ faster-whisper"
pdftotext -v 2>&1 | head -1 && echo "  ✅ pdftotext" || echo "  ❌ pdftotext"
ffmpeg -version 2>/dev/null | head -1 && echo "  ✅ ffmpeg" || echo "  ❌ ffmpeg"

echo ""
echo "--- Layer 5: Automation ---"
crontab -l 2>/dev/null | grep -q "morning-briefing" && echo "  ✅ cron jobs" || echo "  ❌ cron jobs"
which chezmoi 2>/dev/null && echo "  ✅ chezmoi" || echo "  ❌ chezmoi"

echo ""
echo "========================================"
echo " QUICK START COMMANDS"
echo "========================================"
echo ""
echo "  morning         → AI morning briefing"
echo "  evening         → AI evening review"
echo "  td              → Task dashboard"
echo "  n 'note'        → Quick capture"
echo "  ns 'query'      → Search all notes"
echo "  seek 'concept'  → Semantic search"
echo "  wisdom          → Pipe text to extract insights"
echo "  quiz            → Pipe text to generate quiz"
echo "  study           → Pipe text to create study notes"
echo "  ws proj task    → Start time tracking"
echo "  wp              → Stop time tracking"
echo "  process-lecture file.mp4 'Title' notebook"
echo "  process-paper file.pdf 'Title'"
echo "  project-status projectname"
echo "  smart-add description of task"
echo ""
echo "========================================"
```

### Stop the Setup Timer

```bash
watson stop
watson report --day
echo ""
echo "🎉 CLI-AI ARSENAL IS OPERATIONAL."
echo "Run 'morning' tomorrow to see your first AI briefing."
```

---

## First Day Checklist

After installation, do these to build muscle memory:

- [ ] `td` — check your task dashboard
- [ ] `ta "Read Chapter 8 of ML textbook" project:degree-cs priority:M due:wednesday` — add a real task
- [ ] `ws degree-cs "Reading Ch.8"` — start timing
- [ ] `n "First real note: started using CLI-AI Arsenal"` — capture a note
- [ ] `echo "Explain backpropagation" | wisdom` — test AI pipeline
- [ ] `echo "Explain gradient descent" | quiz` — test quiz generation
- [ ] `seek "machine learning optimization"` — test semantic search
- [ ] `wp` — stop timer
- [ ] `wrw` — check weekly time report
- [ ] `evening` — run evening review

---

## Troubleshooting

| Problem | Fix |
|---|---|
| `fabric: command not found` | `echo 'export PATH=$PATH:$(go env GOPATH)/bin' >> ~/.bashrc && source ~/.bashrc` |
| `llm` API errors | Check key: `llm keys` — re-set with `llm keys set gemini` |
| `nb: command not found` | Try `npm install -g nb.sh` or install from GitHub |
| Ollama won't start | `systemctl start ollama` or `ollama serve &` |
| Semantic search returns nothing | Rebuild index: `reindex` |
| `faster-whisper` too slow | Use smaller model: `--model small` or `--model tiny`. GPU recommended for `large-v3`. |
| MCP not working in Gemini | Check `~/.gemini/settings.json` syntax. Ensure `npx` works: `npx --version`. |
| `watson` timezone issues | Set `export TZ=your/timezone` in `~/.bashrc` |
| tmux sessions lost | WSL-specific: sessions die on Windows shutdown. Use `tmux-resurrect` plugin to save/restore. |

---

*Setup complete. Go build something extraordinary.*
